---
title: import ffmpeg in Android
date: 2025-03-26
keywords: c++, Android, ffmpeg
---
我的模擬器是x86_64，所以以下我都用x86_64的ABI作為示範。  
編譯完應該有以下目錄，includes包含以下目錄，裡面都是head files。
```
.
├── includes
│   └── x86_64
│       ├── libavcodec
│       ├── libavfilter
│       ├── libavformat
│       ├── libavutil
│       ├── libswresample
│       └── libswscale
├── libs
│   └── x86_64
├── pkgconfig
│   └── x86_64
└── share
    └── ffmpeg
        └── examples
```
libs目錄包含以下靜態庫。
```
.
└── x86_64
    ├── libavcodec.a
    ├── libavfilter.a
    ├── libavformat.a
    ├── libavutil.a
    ├── libswresample.a
    └── libswscale.a
```
建立一個新的native c的project，目錄結構如下圖  
![img]({{site.imgurl}}/ffmpeg/import_ffmpeg1.png)
1. 新建include目錄，把ffmpeg編譯好的include/x86_64下的目錄全拷貝過來
```
你的android專案位置/app/src/main/cpp/include
```
2. 新建libs目錄，把ffmpeg編譯好的libs目錄下，把x86_64拷貝到以下路徑。
```
你的android專案位置/app/src/main/cpp/libs
```
app下的build.gradle  
增加你要編譯的ABI，本範例為x86_64  
```
android {
	...
    defaultConfig {
        ...
        ndk{
            abiFilters 'x86_64'
        }
        ...
    }
    ...
}
```

CMakeLists.txt
```
cmake_minimum_required(VERSION 3.22.1)
project("myapplication")
add_library(${CMAKE_PROJECT_NAME} SHARED
        # List C/C++ source files with relative paths to this CMakeLists.txt.
        native-lib.cpp)
#匯入head file目錄
include_directories(${CMAKE_SOURCE_DIR}/include)
#給c++編譯器設定libs的目錄
set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -L${CMAKE_SOURCE_DIR}/libs/${ANDROID_ABI}" )
target_link_libraries(${CMAKE_PROJECT_NAME}
        android
        # 要用到的lib，libavcodec去掉lib後面的avcodec就是要用lib
        avcodec avfilter avformat avutil swresample swscale
        log)
```
測試lib是否能include，可以用av_version_info()函式
native-lib.cpp
{% highlight c++ linenos %}
//因為ffmpeg是由c開發，所以要包在extern c{}之間
extern "C"{
#include <libavcodec/avcodec.h>
}
extern "C" JNIEXPORT jstring JNICALL
Java_com_example_myapplication_MainActivity_stringFromJNI(
        JNIEnv* env,
        jobject /* this */) {
    std::string hello = "Hello from C++";
    return env->NewStringUTF(av_version_info());
}
{% endhighlight %}