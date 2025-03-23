---
title: cmake
date: 2025-02-18
keywords: java, ndk, android, cmake
---

## cpu不同
在linux下可以使用以下語句產生執行檔
```
g++ -o hello hello.cpp
```
但hello執行檔可以在Linux下執行，無法在Android手機執行，原因是Linux與Android的操作系統os不同，Android手機的os是ARM架構。

因此編譯Android os的執行檔需要使用ARM的編譯器編譯出.so(動態庫)或.a(靜態庫)，才能在Android的os上執行。

### 查詢模擬器CPU架構
MAC的Android Studio是根據MAC的晶片預設模擬器CPU架構  
1. x86 或 x86_64 → 適合 Intel/AMD CPU，模擬效能較佳（支援 Hypervisor）。
2. arm64-v8a 或 armeabi-v7a → 適合 M1/M2 Mac 或某些 ARM 虛擬機，但效能較低。
3. 如果你在 Apple Silicon（M1/M2/M3）Mac 上運行模擬器，預設會使用 ARM 版的模擬器（arm64-v8a）。

查詢CPU架構  
1. 確保模擬器是運行
2. 進入platform-tools
3. 執行以下語句
```
$ cd ~/Library/Android/sdk/platform-tools
$ ./adb shell uname -m
x86_64
```
顯示結果，我的模擬器是x86_64

## clang
是一個c,c++,Object-c的輕量編譯器，基於LLVM(LLVM是以c++編寫而成的編譯器)

## cmake使用方法
camke的目的是將c++檔案變成Android下可以執行的so檔，so檔是動態庫  
cmake的使用方法，當作函式呼叫，參數以空格分開  
```
函式(參數1 參數2 參數3)
```

也可以用斷行分開  
```
函式(參數1
參數2
參數3)
```

## cmake目錄路徑
放置cmake文件的目錄
```
${CMAKE_SOURCE_DIR}
```

## 顯示變數
CMakeLists.txt
```
message("Android_ABI: ${ANDROID_ABI}")
```

## 設定版本
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.22.1)
```

## 原始檔案變so檔
ndkproj是so檔的名字，會產生ndkproj.so的檔案  
SHARED是動態庫  
native-lib.cpp是原始檔案  
CMakeLists.txt  
```
add_library( # Sets the name of the library.
        ndkproj

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        native-lib.cpp)
```

## 尋找log
如果沒有設定ndkVersion  
預設會在local.properties下尋找sdk的安裝目錄  
`sdk.dir=/Users/cici/Library/Android/sdk`中的ndk目錄下尋找log庫，找到的path，設給log-lib這個變數  
CMakeLists.txt
```
find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)
```

## 自行編譯動態庫與靜態庫
### ndk路徑
我的ndk路徑如下
```
/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/bin/x86_64-linux-android33-clang
```
### CMakeLists.txt路徑
我的CMakeLists.txt路徑如下
```
/Users/cici/AndroidStudioProjects/ndkProj/app/src/main/cpp
```
### 建立jniLibs目錄
在以下的路徑下建立jniLibs目錄
```
/Users/cici/AndroidStudioProjects/ndkProj/app/src/main
```

### c檔案
檔名testc.c
{% highlight c++ linenos %}
int test() {
  return 1;
}
{% endhighlight %}

### build.gradle(app)設定
#### 專案的ABI
設置自己寫的ndk產生各種cpu的so檔，可多個，目前範例只有'x86_64'
{% highlight c++ linenos %}
android {
	...
	defaultConfig {
		...
        externalNativeBuild {
            ndkBuild {
                abiFilters 'x86_64'
            }
        }
		...
	}
	...
}
{% endhighlight %}

#### 第三方的Lib的ABI
所謂第三方的Lib是指(dependencies)依賴的Lib
{% highlight c++ linenos %}
dependencies {
	implementation '....'
}
{% endhighlight %}

設置第三方的Lib產生各種cpu的so檔，可多個，目前範例只有'x86_64'
{% highlight c++ linenos %}
android {
	...
	defaultConfig {
		...
        ndk {
            abiFilters  'x86_64'
        }
    }
	...
}
{% endhighlight %}

#### cmake文件路徑
cmake文件放置路徑跟build.gradle(app)同目錄下的`src/main/cpp`
{% highlight c++ linenos %}
android {
	...
    externalNativeBuild {
        cmake {
            path file('src/main/cpp/CMakeLists.txt')
            version '3.22.1'
        }
    }
    ...
}
{% endhighlight %}

![img]({{site.imgurl}}/cmake/cmake_path_setting.png)

#### jniLibs設定
so檔放置路徑
{% highlight c++ linenos %}
android {
	...
    sourceSets {
        main {
            jniLibs.srcDirs = ['src/main/jniLibs']
        }
    }
    ...
}
{% endhighlight %}

### 編譯so
終端機執行以下指令，產生出libTest.so
```
/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/bin/x86_64-linux-android33-clang --sysroot=/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/sysroot -fPIC -shared testc.c -o libTest.so
```

#### 匯入動態態庫
CmakeList.txt
```
#動態庫
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L${CMAKE_CURRENT_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}")
```
解釋

1. `..`代表回上一層目錄
2. `${CMAKE_CXX_FLAGS}`CMakeLists.txt放置的目錄
3. `${ANDROID_ABI}`代表abiFilters，目前是x86_64

#### 檢查so檔的ABI
```
$ file libTest.so
libTest.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, not stripped
```

### 編譯靜態庫
#### 先產生.o的檔案
```
/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/bin/x86_64-linux-android33-clang --sysroot=/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/sysroot -fPIC -c testc.c -o testc.o
```

#### 產生.a靜態庫
```
/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/bin/llvm-ar -r libTest.a testc.o
```
產生出libTest.a

#### 匯入靜態庫
libTest.a放在跟cmake.md同一個目錄下  
CMakeLists.txt
```
add_library(Test STATIC IMPORTED)
set_target_properties(Test PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libTest.a)

```

### 導入到專案中
MainActivity.java增加
{% highlight c++ linenos %}
    static {
        System.loadLibrary("ndkproj");
        System.loadLibrary("Test");
    }
{% endhighlight %}

#### 鏈結
1. 注意！ndkproj是要產生的project的so，所以一定要放在第一個，不能把Test放在最上面
2. 需要編譯ndkproj.so需要依賴log-lib，需要Test的library
3. 靜態檔:Test與add_library()中的Test是要一模一樣
4. 動態檔:會自動尋找libTest.so，根據`Test`，前面加上lib，以及副檔名.so

CMakeLists.txt
```
target_link_libraries( # Specifies the target library.
        ndkproj

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib}
        Test
        )
```

## jni使用外部的.so檔
### extern c
呼叫c檔案的function，要用`extern "C"{}`包起來。  
{% highlight c++ linenos %}
extern "C" {
int test();
}
{% endhighlight %}

### android log head file
使用Logcat可以看到錯誤訊息  
{% highlight c++ linenos %}
#include <android/log.h>
{% endhighlight %}

### 使用android log
{% highlight c++ linenos %}
    int result = test();

    // 使用 __android_log_print() 輸出到 Logcat
    __android_log_print(ANDROID_LOG_DEBUG, "NDK_TEST", "test() 返回值: %d", result);
{% endhighlight %}

### 完整檔案
MainActivity.java
{% highlight c++ linenos %}
public class MainActivity extends AppCompatActivity {

    // Used to load the 'ndkproj' library on application startup.
    static {
        System.loadLibrary("ndkproj");
        System.loadLibrary("Test");
    }

    private ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        // Example of a call to a native method
        TextView tv = binding.sampleText;
        tv.setText(stringFromJNI() + stringFromJNI2("abcdefg") + _test());
    }
    public native String stringFromJNI();
}
{% endhighlight %}

native-lib.cpp
{% highlight c++ linenos %}
#include <jni.h>
#include <string>
#define JNI_CLASS_PATH "com/example/ndkproj/MainActivity"
#include <android/log.h>
// 告知編譯器 test() 是一個 C 語言函式，避免名稱修飾
extern "C" {
int test();
}
extern "C" JNIEXPORT jstring JNICALL
Java_com_example_ndkproj_MainActivity_stringFromJNI(
        JNIEnv* env,
        jobject /* this */) {
    int result = test();
    // 使用 __android_log_print() 輸出到 Logcat
    __android_log_print(ANDROID_LOG_DEBUG, "NDK_TEST", "test() 返回值: %d", result);
    std::string hello = "Hello from C++";
    return env->NewStringUTF(hello.c_str());
}
{% endhighlight %}

CMakeLists.txt
{% highlight c++ linenos %}
cmake_minimum_required(VERSION 3.22.1)
project("ndkproj")
add_library(ndkproj SHARED native-lib.cpp)
#靜態庫
add_library(Test STATIC IMPORTED)
set_target_properties(Test PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libTest.a)
#動態庫
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L${CMAKE_CURRENT_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}")
find_library(log-lib log)
link_directories(${CMAKE_SOURCE_DIR})
target_link_libraries(ndkproj ${log-lib} Test)
{% endhighlight %}