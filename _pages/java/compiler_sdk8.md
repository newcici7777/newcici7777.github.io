---
title: Compiler JDK8
date: 2025-05-26
keywords: Java, Compiler JDK8
---
## 下載jdk8
開啟終端機，在Downloads目錄下，執行以下句子。
```
git clone https://github.com/openjdk/jdk8u
```

## 安裝一些工具
```
# 安装基础编译工具和依赖
brew install autoconf freetype ccache pkg-config

xcode-select --install  # 确保Xcode命令行工具已安装
```

binutils
```
brew install binutils

echo 'export PATH="/usr/local/opt/binutils/bin:$PATH"' >> ~/.zshrc

# 立即生效
source ~/.zshrc

```

## 下載jdk7
為什麼要下載jdk7呢？

編譯的時候需要jdk8的前一個版本的jdk。

我是mac，我下載的bin目錄的位置在此處。
```
/Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home
```

## 暫時設定Java Home
```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home
## 檢查版本
$JAVA_HOME/bin/java -version
```

要確保印出以下內容
```
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
```

## 編譯
進到jdk8的目錄
```
cd jdk8u
```

```
# 确保使用绝对路径
export OBJCOPY=$(which objcopy)

# 完整配置命令（必须在一行）
bash configure \
    --with-boot-jdk=$JAVA_HOME \
    --with-debug-level=slowdebug \
    --enable-debug-symbols \
    --with-native-debug-symbols=internal \
    OBJCOPY=$OBJCOPY
```

## make
```
make JOBS=2 images
```

## 若要重新make，請先clean
```
make clean
rm -rf build
```

## 檢查PrintVtables功能
檢查是否可以用PrintVtables功能
```
 ./build/macosx-x86_64-normal-server-slowdebug/jdk/bin/java -XX:+UnlockDiagnosticVMOptions -XX:+PrintVtables 
```

## JAVA_HOME
把build目錄中的編譯好的macosx-x86_64-normal-server-slowdebug，更改名jdk8_debug

修改zshrc
```
vi ~/.zshrc
```

```
export JAVA_HOME=/Users/cici/Downloads/jdk8u/build/jdk8_debug/jdk
export PATH=$JAVA_HOME/bin:$PATH
```

```
source ~/.zshrc
```

## 檢查版本
```
java -version 
openjdk version "1.8.0_462-internal-debug"
OpenJDK Runtime Environment (build 1.8.0_462-internal-debug-cici_2025_05_27_10_21-b00)
OpenJDK 64-Bit Server VM (build 25.462-b00-debug, mixed mode)
```

## 測試
```
java -XX:+UnlockDiagnosticVMOptions -XX:+PrintVtables PolyExample
```

## 測試檔案
{% highlight java linenos %}
class Animal {
    public void speak() { System.out.println("Animal"); }
    public void eat() { System.out.println("Animal eats"); }
}

class Dog extends Animal {
    @Override public void speak() { System.out.println("Dog"); }
    @Override public void eat() { System.out.println("Dog eats"); }
}

class SuperDog extends Dog {
    @Override public void speak() { System.out.println("SuperDog"); }
    @Override public void eat() { System.out.println("SuperDog eats"); }
}

public class TestVtable {
    public static void main(String[] args) throws Exception {
        for (int i = 0; i < 10000; i++) {
            Animal a = getAnimal(i);
            a.speak();
            a.eat();
        }
        Thread.sleep(300_000); // 等待觀察
    }

    public static Animal getAnimal(int i) {
        if (i % 2 == 0) return new SuperDog();
        else return new Dog();
    }
}
{% endhighlight %}

## 編譯
```
./build/macosx-x86_64-normal-server-slowdebug/jdk/bin/javac TestVtable
```

## 執行
```
./build/macosx-x86_64-normal-server-slowdebug/jdk/bin/java \
  -Xcomp \
  -XX:+UnlockDiagnosticVMOptions \
  -XX:+PrintVtables \
  -XX:+TraceClassLoading \
  -XX:+TraceClassInitialization \
  TestVtable 2>&1 | tee vtables.log
```

## 檔案中尋找vtable
```
grep -i vtable vtables.log
grep -A10 'Vtable for class' vtables.log
```
