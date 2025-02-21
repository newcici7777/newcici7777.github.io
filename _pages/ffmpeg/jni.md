---
title: JNI
date: 2025-02-05
keywords: java, jni, android
---

6788

## JNI

JNI是指程式運行時Java程式碼可以使用C或C++的lib，也可以在C或C++的lib使用Java程式碼。

## NDK

是一個工具，包含Android的交叉編譯器，Android可以用的靜態庫與動態庫。

## 建立Java可以呼叫的C++

### java sdk

我是MAC電腦，在終端機執行以下指令，就會顯示java sdk的路徑
```
$ /usr/libexec/java_home
/Users/cici/Library/Java/JavaVirtualMachines/openjdk-20.0.1/Contents/Home
```

### native

MainActivity.java
{% highlight c++ linenos %}
public native String stringFromJNI();
{% endhighlight %}

在函式前面加上native，表示這個是呼叫c++函式。

### jni.h
native-lib.cpp
{% highlight c++ linenos %}
#include <jni.h>
{% endhighlight %}

先include jni.h的head file

### extern c
native-lib.cpp
{% highlight c++ linenos %}
extern "C" JNIEXPORT jstring JNICALL
{% endhighlight %}

若是c++寫的程式，前面都要加上extern c，Java才可以辦識這是C++程式

### 傳回值

MainActivity.java
{% highlight c++ linenos %}
public native String stringFromJNI();
{% endhighlight %}
java函式傳回值是String

native-lib.cpp
{% highlight c++ linenos %}
extern "C" JNIEXPORT jstring JNICALL
{% endhighlight %}
c++函式傳回值的型態是jstring

### Java的型態與c++型態
|Java型態|c++型態|
|:--:|:--:|
|void   |void   |
|boolean|jboolean|
|byte	|jbyte	|
|char	|jchar  |
|short	|jshort |
|int	|jint	|
|long	|jlor	|
|float	|jfloat	|
|double	|jdouble|	
|Object	|jobject|	
|Class	|jclass	|
|String	|jstring|	
|Object[]	|jobjectArray |
|boolean[]	|jbooleanArray|
|byte[]	    |jbyteArray   |
|char[]	    |jcharArray   |
|short[]	|jshortArray  |
|int[]	    |jintArray    |
|long[]	    |jlongArray   |
|float[]	|jfloatArray  |
|double[]	|jdoubleArray |

### jni函式參數
以下是c++中的jni函式
{% highlight c++ linenos %}
Java_com_example_ndkproj_MainActivity_stringFromJNI(
        JNIEnv* env,
        jobject thiz) {
}
{% endhighlight %}
#### JNIEnv
Java虛擬機

#### jobject instance
呼叫c++函式的類別，以本頁的例子是MainActivity.java


## 完整程式碼

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

    /**
     * A native method that is implemented by the 'ndkproj' native library,
     * which is packaged with this application.
     */
    public native String stringFromJNI();
    public native String stringFromJNI2(String str);
    public native String _test();
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


extern "C"
JNIEXPORT jstring JNICALL
Java_com_example_ndkproj_MainActivity_stringFromJNI2(JNIEnv *env, jobject thiz,
                                                     jstring _str) {
    const char* str = env->GetStringUTFChars(_str, 0);
    env->ReleaseStringUTFChars(_str, str);
    return env->NewStringUTF("test");
}

extern "C"
JNIEXPORT jstring JNICALL
my_test_register(JNIEnv *env,
                 jobject instance) {
    return env->NewStringUTF("this is a test of register!");
}
static JNINativeMethod g_methods[] = {
        {
            "_test",
            "()Ljava/lang/String;",
            (void*)my_test_register
        },
};
jint JNI_OnLoad(JavaVM *vm, void *reserved) {
    JNIEnv *env = NULL;
    vm->GetEnv((void**)&env, JNI_VERSION_1_6);
    jclass clazz = env->FindClass(JNI_CLASS_PATH);
    env->RegisterNatives(clazz, g_methods, sizeof(g_methods)/sizeof(g_methods[0]));
    return JNI_VERSION_1_6;
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