---
title: Compiler SDK
date: 2025-05-26
keywords: Java, Compiler SDK
---
## 下載jdk17u
開啟終端機，在Downloads目錄下，執行以下句子。
```
git clone https://github.com/openjdk/jdk17u
```

## 安裝一些必要的指令
```
# 安装基础编译工具和依赖
brew install autoconf freetype ccache pkg-config

xcode-select --install  # 确保Xcode命令行工具已安装
```

## 下載zulu
未避免遇到以下問題
```
configure: Could not find a valid Boot JDK. 
configure: This might be fixed by explicitely setting --with-boot-jdk
configure: error: Cannot continue
/Users/cici/Downloads/jdk8u/common/autoconf/generated-configure.sh: line 82: 5: Bad file descriptor
```
下載zulu

```
# 下載在Downloads目錄下
curl -LO https://cdn.azul.com/zulu/bin/zulu16.30.15-ca-jdk16.0.1-macosx_x64.tar.gz

## 解壓
tar -xzf zulu16.30.15-ca-jdk16.0.1-macosx_x64.tar.gz

## 設置臨時JAVA_HOME，非永久
export JAVA_HOME=$(pwd)/zulu16.30.15-ca-jdk16.0.1-macosx_x64

## 檢查版本
$JAVA_HOME/bin/java -version
````
確保執行結果為以下內容
```
openjdk version "16.0.1" 2021-04-20
OpenJDK Runtime Environment Zulu16.30+15-CA (build 16.0.1+9)
OpenJDK 64-Bit Server VM Zulu16.30+15-CA (build 16.0.1+9, mixed mode, sharing)
```

## configure
進到jdk17u目錄
```
bash configure \
    --with-debug-level=fastdebug \
    --with-boot-jdk=$JAVA_HOME
```

## make
進到jdk17u目錄
```
make JOBS=2 images
```

確保出現以下句子
```
Finished building target 'images' in configuration 'macosx-x86_64-server-fastdebug'
```

## 使用PrintVtableStats
進到jdk17u目錄
```
./build/macosx-x86_64-server-fastdebug/jdk/bin/java -XX:+UnlockDiagnosticVMOptions -XX:+PrintVtableStats -version | grep -i vtable 
 ```

結果如下
```
openjdk version "17.0.16-internal" 2025-07-15
OpenJDK Runtime Environment (fastdebug build 17.0.16-internal+0-adhoc.cici.jdk17u)
OpenJDK 64-Bit Server VM (fastdebug build 17.0.16-internal+0-adhoc.cici.jdk17u, mixed mode)
vtable statistics:
  7664 bytes fixed overhead (refs + vtable object header)
 87360 bytes for vtable entries (8120 for arrays)
 ```

./build/macosx-x86_64-server-fastdebug/jdk/bin/javac -d . vtable_test/PolyExample.java
./build/macosx-x86_64-server-fastdebug/jdk/bin/java -XX:+UnlockDiagnosticVMOptions vtable_test.PolyExample
Bark!
Press Enter to exit...

jps -l | grep PolyExample
99284 vtable_test.PolyExample

./build/macosx-x86_64-server-fastdebug/jdk/bin/jhsdb clhsdb --pid 1708

dumpheap：导出堆内存到文件（需后工具分析）。

print：打印对象字段值（更高级抽象）。
-------------------------------------
mem
两种查询方式：

单地址 + 长度：address/count

从 address 开始，显示 count 个字节。

地址范围：address,address

显示两个地址之间的内存。

你输入的 mem 0x00000008000c1228+B8 0x20 有两个问题：

不支持 +B8 偏移量语法：必须手动计算地址（如 0x00000008000c1228 + 0xB8 = 0x00000008000c12e0）。

第二个参数 0x20 格式错误：长度需要用 / 分隔，而不是空格。0x20不支援，要用32
hsdb> mem 0x00000008000c1228/32

class Dog
hsdb> universe  # 确认堆内存状态（你已执行）
hsdb> classes           # 列出所有已加载的类
hsdb> class Dog 
Dog @0x00000008000c1228  
hsdb> inspect 0x00000008000c1228 # inspect 查看对象结构，再用 mem 分析原始内存：
hsdb> dumpclass 0x00000008000c1228




Array<Method*>* InstanceKlass::_methods: Array<Method*> @ 0x000000011e40e958

inspect 0x000000011e40ea58
