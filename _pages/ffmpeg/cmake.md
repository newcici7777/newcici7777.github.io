---
title: cmake
date: 2025-02-18
keywords: java, ndk, android, cmake
---
## 自行編譯動態庫與靜態庫

我的ndk路徑如下
```
/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/bin/aarch64-linux-android24-clang
```

### 編譯so

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  cout << "Hello World!" << endl;
}
{% endhighlight %}

終端機執行以下指令
```
/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/bin/aarch64-linux-android24-clang --sysroot=/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/sysroot -fPIC -shared testc.cpp -o libTest.so
```

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


## 設定版本
```
cmake_minimum_required(VERSION 3.22.1)
```

## 原始檔案變so檔

ndkproj是so檔的名字，會產生ndkproj.so的檔案

SHARED是動態庫

native-lib.cpp是原始檔案

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
```
find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)
```

## 匯入靜態庫
libTest.a放在跟cmake.md同一個目錄下
```
add_library(Test STATIC IMPORTED)
```