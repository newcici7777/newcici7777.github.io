---
title: JNI
date: 2025-02-05
keywords: java, jni, android
---
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
在函式前面加上native，表示這個是呼叫c++函式。  
MainActivity.java
{% highlight c++ linenos %}
public native String stringFromJNI();
{% endhighlight %}

### c++函式與java函式相互跳躍
![img]({{site.imgurl}}/ndk/jump_to_c.png)

![img]({{site.imgurl}}/ndk/jump_to_java.png)

### jni.h
先include jni.h的head file  
native-lib.cpp
{% highlight c++ linenos %}
#include <jni.h>
{% endhighlight %}

### extern c
若是c++寫的程式，前面都要加上extern c，Java才可以辦識這是C++程式  
native-lib.cpp
{% highlight c++ linenos %}
extern "C" JNIEXPORT jstring JNICALL
{% endhighlight %}

### 傳回值
c++函式傳回值的型態是jstring  
MainActivity.java
{% highlight c++ linenos %}
public native String stringFromJNI();
{% endhighlight %}
java函式傳回值是String

native-lib.cpp
{% highlight c++ linenos %}
extern "C" JNIEXPORT jstring JNICALL
{% endhighlight %}

### Java的型態與c++型態

|Java類別|jni類別|
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
代表Java虛擬機，透過Java虛擬機可以取得Java類別物件。

#### jobject instance
呼叫c++函式的類別，以本頁的例子是MainActivity.java

#### 動態庫so檔
project編譯完成會打包成.so檔，系統啟動的時候會去把專案ndkproj.so Load進來
{% highlight c++ linenos %}
public class MainActivity extends AppCompatActivity {
    static {
        System.loadLibrary("ndkproj");
    }
    ...
}
{% endhighlight %}    

#### jni取得字串
MainActivity.java
{% highlight c++ linenos %}
public class MainActivity extends AppCompatActivity {
    static {
        System.loadLibrary("ndkproj");
    }
    private ActivityMainBinding binding;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        // Example of a call to a native method
        TextView tv = binding.sampleText;
        tv.setText(stringFromJNI() + stringFromJNI2("abcdefg"));
    }
    public native String stringFromJNI();
    public native String stringFromJNI2(String str);
}
{% endhighlight %}

native-lib.cpp
{% highlight c++ linenos %}
#include <jni.h>
#include <string>
#include <android/log.h>
extern "C" JNIEXPORT jstring JNICALL
Java_com_example_ndkproj_MainActivity_stringFromJNI(
        JNIEnv* env,
        jobject /* this */) {
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
{% endhighlight %}

## jni陣列類型轉型
MainActivity.java
{% highlight c++ linenos %}
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ...
        int intArr[] = {11, 22, 33, 44};
        String strArr[] = {"test1", "test2", "test3"};
        test2(1, "Hello World", intArr, strArr);
        Log.e("Java", Arrays.toString(intArr));
    }
{% endhighlight %}

native-lib.cpp要include Android寫Log的head file
```
#include <android/log.h>
// ...代表__VA_ARGS__，可變參數
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, "JNI",__VA_ARGS__);
```

native-lib.cpp
{% highlight c++ linenos %}
extern "C"
JNIEXPORT void JNICALL
Java_com_example_ndkproj_MainActivity_test2(JNIEnv *env, jobject thiz, jint num, jstring str,
                                            jintArray int_arr, jobjectArray str_arr) {
    // 取得陣列指標
    jint *int_arr_p = env->GetIntArrayElements(int_arr, NULL);
    // 取得長度
    int32_t len = env->GetArrayLength(int_arr);
    for (int i = 0; i < len; i++) {
        // 參數1:Log級別
        // 參數2:TAG
        // 參數3:印出的值
        // 參數4:把值傳給參數3
        //__android_log_print(ANDROID_LOG_ERROR, "JNI", "取得值%d", *(int_arr_p + i));
        LOGE("取得值%d", *(int_arr_p + i));
        // 修改值
        *(int_arr_p + i) = *(int_arr_p + i) + 100;
    }
    // 使用GetIntArrayElements，就要釋放記憶體空間
    // 釋放記憶體空間
    // 參數1: java傳來的int_arr
    // 參數2: 由GetIntArrayElements得到的指標
    // 參數3: 模式
    // 0 : 更新java傳來的int_arr，並且釋放指標int_arr記憶體空間
    // 1 : 只更新java傳來的int_arr
    // 2 : 只釋放指標int_arr記憶體空間
    env->ReleaseIntArrayElements(int_arr, int_arr_p, 0);
    jint strlen = env->GetArrayLength(str_arr);
    for (int i = 0; i < strlen; i++) {
        jstring str = static_cast<jstring>(env->GetObjectArrayElement(str_arr, i));
        // 將jstring轉成c++
        const char* s = env->GetStringUTFChars(str, 0);
        LOGE("取得值%s", s);
        // release memory
        env->ReleaseStringUTFChars(str, s);
    }
}
{% endhighlight %}

## Android Studio Log
![img]({{site.imgurl}}/other/logcat1.png)
![img]({{site.imgurl}}/other/logcat2.png)

## jni簽名

|Java類別|簽名|
|boolean |Z|
|short   |S|
|float   |F|
|byte    |B|
|int     |I|
|double  |D|
|char    |C|
|long    |J|
|void    |V|
|String |Ljava/lang/String;|
|Array  |[+類別簽名|

## jni取得物件，執行成員函式，修改成員變數
Bean1.java
{% highlight c++ linenos %}
package com.example.ndkproj;
import android.util.Log;
public class Bean {
    private static final String TAG = "Bean";
    private int data;
    public int getData() {
        return data;
    }
    public void setData(int data) {
        Log.e(TAG, "somebody setdata");
        this.data = data;
    }
    public static void printInfo(String msg) {
        Log.e(TAG, msg);
    }
    public static void printInfo(Bean2 bean2) {
        Log.e(TAG, "param is object:" + bean2.i);
    }
}
{% endhighlight %}

Bean2.java
{% highlight c++ linenos %}
package com.example.ndkproj;
public class Bean2 {
  int i = 12345;
  public Bean2() {
  }
  public Bean2(int i) {
    this.i = i;
  }
}
{% endhighlight %}

MainActivity.java
{% highlight c++ linenos %}
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        // Example of a call to a native method
        Bean bean = new Bean();
        bean.setData(5000);
        passObject(bean);
    }
    native void passObject(Bean bean);
{% endhighlight %}

native-lib.cpp
{% highlight c++ linenos %}
extern "C"
JNIEXPORT void JNICALL
Java_com_example_ndkproj_MainActivity_passObject(JNIEnv *env, jobject thiz, jobject bean) {
  // 1. 取得class
  jclass beanCls = env->GetObjectClass(bean);
  // 2. 取得get方法
  // 參數1:class
  // 參數2:函式名
  // 參數3:簽名()I代表無法參數，回傳int
  jmethodID getI = env->GetMethodID(beanCls, "getData", "()I");
  // 3. 呼叫方法
  jint i = env->CallIntMethod(bean, getI);
  LOGE("getI():%d", i);
  // set方法
  // (I)V參數是Int，傳回值是Void
  jmethodID setI = env->GetMethodID(beanCls, "setData", "(I)V");
  env->CallVoidMethod(bean, setI, 1000);
  // static方法
  jmethodID printInfo = env->GetStaticMethodID(beanCls, "printInfo", "(Ljava/lang/String;)V");
  // 建立java 字串
  jstring param1 = env->NewStringUTF("call static method");
  env->CallStaticVoidMethod(beanCls,printInfo,param1);
  // 參數是自定義類別
  // static方法
  jmethodID printInfo2 = env->GetStaticMethodID(beanCls, "printInfo", "(Lcom/example/ndkproj/Bean2;)V");
  // 建立物件jclass
  jclass bean2Cls = env->FindClass("com/example/ndkproj/Bean2");
  // 呼叫空建構子()V
  // 呼叫有int建構子(I)V
  jmethodID constructor = env->GetMethodID(bean2Cls, "<init>", "(I)V");
  // 呼叫建構子取得物件
  jobject bean2 = env->NewObject(bean2Cls, constructor, 1111);
  env->CallStaticVoidMethod(beanCls,printInfo2, bean2);
  // 釋放區域變數
  // new出來的都要釋放記憶體
  // delete方法針對jobject(FindClass,NewObject)
  env->DeleteLocalRef(bean2Cls);
  env->DeleteLocalRef(bean2);
  // 自已建立的物件，包含自己建立的jstring用delete
  env->DeleteLocalRef(param1);
  // release用在GetStringUTFChars
  //env->ReleaseStringUTFChars(str, s);
  // 取得成員變數
  jfieldID jfieldId = env->GetFieldID(beanCls, "data", "I");
  // 修改成員變數
  env->SetIntField(bean, jfieldId, 666);
}
{% endhighlight %}
```
2025-03-18 12:41:58.614 16753-16753 JNI                     com.example.ndkproj                  E  getI():5000
2025-03-18 12:41:58.610 16753-16753 Bean                    com.example.ndkproj                  E  somebody setdata
2025-03-18 12:41:58.616 16753-16753 Bean                    com.example.ndkproj                  E  somebody setdata
2025-03-18 12:41:58.620 16753-16753 Bean                    com.example.ndkproj                  E  call static method
2025-03-18 12:41:58.622 16753-16753 Bean                    com.example.ndkproj                  E  param is object:1111
```

## 記憶體釋放

以下的寫法是在建立指標(請見下一個程式碼jni.h)，使用typedef把指標取成去掉\*的別名，但實際上是指標。
{% highlight c++ linenos %}
jobject bean2 = env->NewObject(bean2Cls, constructor, 1111);
jstring param1 = env->NewStringUTF("call static method");
jclass bean2Cls = env->FindClass("com/example/ndkproj/Bean2");  
{% endhighlight %}

jni.h
{% highlight c++ linenos %}
typedef _jobject*       jobject;
typedef _jclass*        jclass;
typedef _jstring*       jstring;
{% endhighlight %}

jni已經做到在函式中建立指標，離開函式這些指標會自動被JVM釋放記憶體，就算沒寫以下的程式碼，也會被記憶體釋放。
{% highlight c++ linenos %}
  // delete方法針對jobject(FindClass,NewObject)
  env->DeleteLocalRef(bean2Cls);
  env->DeleteLocalRef(bean2);
  // 自已建立的物件，包含自己建立的jstring用delete
  env->DeleteLocalRef(param1);
{% endhighlight %}

## 全域參考與區域指標

### NewGlobalRef
區域參考離開函式就會被記憶體回收，使用全域參考則不會被JVM釋放記憶體。

### Delete GlobalRef
刪除全域參考
```
env->DeleteGlobalRef(global_bean_cls);
```
{% highlight c++ linenos %}
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());
        invokeBean2Method();
        invokeBean2Method();
    }
    native void invokeBean2Method();
{% endhighlight %}

{% highlight c++ linenos %}
// 全域變數
jclass global_bean_cls;
//
jclass weak_bean_cls;
extern "C"
JNIEXPORT void JNICALL
Java_com_example_ndkproj_MainActivity_invokeBean2Method(JNIEnv *env, jobject thiz) {
  if (global_bean_cls == NULL) {
    // 區域變數
    jclass cls = env->FindClass("com/example/ndkproj/Bean2");
    // 建立全域參考
    global_bean_cls = static_cast<jclass>(env->NewGlobalRef(cls));
    // 釋放區域變數
    env->DeleteLocalRef(cls);
    // 釋放全域變數
    //env->DeleteGlobalRef(global_bean_cls);
  }
  jmethodID contructor = env->GetMethodID(global_bean_cls, "<init>", "(I)V");
  jobject bean2 = env->NewObject(global_bean_cls, contructor, 666);
  // 弱參考會被記憶體回收
  // 判斷弱參考是否存在
  jboolean isExist = env->IsSameObject(weak_bean_cls, NULL);
  if (isExist) {
    // 區域變數
    jclass cls = env->FindClass("com/example/ndkproj/Bean2");
    // 弱參考
    weak_bean_cls = static_cast<jclass>(env->NewWeakGlobalRef(cls));
    env->DeleteLocalRef(cls);
    // delete weak
    //env->DeleteWeakGlobalRef(weak_bean_cls);
  }
}
{% endhighlight %}

## 動態註冊jni
之前的都是靜態註冊，缺點是函式名很長，接下來要說明的是動態註冊。

### JNI_OnLoad
project編譯完成會打包成.so檔，系統啟動的時候會去把專案ndkproj.so Load進來
{% highlight c++ linenos %}
public class MainActivity extends AppCompatActivity {
    static {
        System.loadLibrary("ndkproj");
    }
    ...
}
{% endhighlight %}   

Load進來的時候會呼叫JNI_OnLoad()函式
{% highlight c++ linenos %}
jint JNI_OnLoad(JavaVM *vm, void *reserved) {
    return JNI_VERSION_1_6;
}
{% endhighlight %}   
第一個參數\*vm是虛擬機，第二個參數可以不用管它。  
最後一定要傳回JNI_VERSION_1_6  

### g_method[]
是一個陣列，陣列裡包含java的函式(第1個參數)與函式簽名(第2個參數)，c++的函式名(第3個參數)，記得c++函式名前面一定要加上(void\*)
{% highlight c++ linenos %}
static JNINativeMethod g_methods[] = {
  {"_test","()Ljava/lang/String;",(void*)my_test_register},
};
{% endhighlight %}

以下是java的native函式名_test()，函式簽名是`()Ljava/lang/String;`傳回值是String物件，沒參數。  
MainActivity.java
{% highlight c++ linenos %}
public class MainActivity extends AppCompatActivity {
  ...
  public native String _test();
  ...
}
{% endhighlight %}

以下是C++函式要與java函式對映。  
參數1是java虛擬機要寫  
參數2是呼叫c++函式的類別，以本頁的例子是MainActivity.java  
native-lib.cpp
{% highlight c++ linenos %}
extern "C"
JNIEXPORT jstring JNICALL
my_test_register(JNIEnv *env,
                 jobject instance) {
    return env->NewStringUTF("this is a test of register!");
}
{% endhighlight %}

### 取得env
第一個參數&env指標參考為要轉為(void\*\*)，第二個參數一定要是JNI_VERSION_1_6
{% highlight c++ linenos %}
jint JNI_OnLoad(JavaVM *vm, void *reserved) {
    // 空的env指標
    JNIEnv *env = NULL;
    // 取得env，第一個參數env指標的位址
    vm->GetEnv((void**)&env, JNI_VERSION_1_6);
    ...
    return JNI_VERSION_1_6;
}
{% endhighlight %} 

若要檢查是否有取得到java虛擬機，可用以下程式碼。  
{% highlight c++ linenos %}
    // ret = 0 JNI_OK 成功
    // ret < 0 JNI_ERR 失敗
    int ret = vm->GetEnv((void**)&env, JNI_VERSION_1_6);
    if (ret != JNI_OK) {
      // crush
      return -1;
    }
{% endhighlight %} 

### 取得g_method[]長度
以下語法取得g_method長度
```
sizeof(g_methods)/sizeof(g_methods[0])
```

### clazz
{% highlight c++ linenos %}
#define JNI_CLASS_PATH "com/example/ndkproj/MainActivity"
{% endhighlight %} 

或者寫成以下方式也可以
{% highlight c++ linenos %}
static const char* clazz_path = "com/example/ndkproj/MainActivity";
{% endhighlight %} 

{% highlight c++ linenos %}
    // java函式的所在的類別
    jclass clazz = env->FindClass(JNI_CLASS_PATH);
{% endhighlight %} 

### 註冊
- 參數1是java函式的類別
- 參數2是g_methods
- 參數3是g_methods的長度
{% highlight c++ linenos %}
env->RegisterNatives(clazz, g_methods, sizeof(g_methods)/sizeof(g_methods[0]));
{% endhighlight %}

### 完整程式碼
MainActivity.java
{% highlight c++ linenos %}
public class MainActivity extends AppCompatActivity {

    // Used to load the 'ndkproj' library on application startup.
    static {
        System.loadLibrary("ndkproj");
    }

    private ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        // Example of a call to a native method
        TextView tv = binding.sampleText;
        tv.setText(_test());
    }
    public native String _test();
}
{% endhighlight %}

native-lib.cpp
{% highlight c++ linenos %}
#include <jni.h>
#include <string>
#define JNI_CLASS_PATH "com/example/ndkproj/MainActivity"
#include <android/log.h>
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

## JNI執行緒

MainActivity.java
{% highlight c++ linenos %}
public class MainActivity extends AppCompatActivity {
    // Used to load the 'ndkproj' library on application startup.
    static {
        System.loadLibrary("ndkproj");
    }
    private ActivityMainBinding binding;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());
        testThread();
    }
    public void updateUI() {
        // 主執行緒 main thread
        if (Looper.myLooper() == Looper.getMainLooper()) {
            Toast.makeText(this, "update UI", Toast.LENGTH_SHORT).show();
        } else {
            // 非主執行緒
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, "update ui2", Toast.LENGTH_SHORT).show();
                }
            });
        }
    }
    native void testThread();
}
{% endhighlight %}

native-lib.cpp
{% highlight c++ linenos %}
#include <jni.h>
#include <string>
#include <android/log.h>
#include <pthread.h>
// ...代表__VA_ARGS__，可變參數
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, "JNI",__VA_ARGS__);
// 全域變數
JavaVM* _vm = nullptr;
jint JNI_OnLoad(JavaVM *vm, void *reserved) {
  // 透過JNI_OnLoad取得jvm
  _vm = vm;
  return JNI_VERSION_1_6;
}
// 建立結構體
struct Context {
  // instance為mainActivity放置java函式的類別
  jobject instance;
};
void* threadTask(void* args) {
  JNIEnv *env;
  // env本來就不是多執行緒，要變成可被多執行緒使用，要用jvm中的AttachCurrentThread
  jint rtn = _vm->AttachCurrentThread(&env, 0);
  // 判斷有沒有把env加進執行緒中
  if (rtn != JNI_OK) {
    return 0;
  }
  // 參數是傳進執行緒中的context，將void*轉成Context
  Context *context = static_cast<Context*>(args);
  jclass cls = env->GetObjectClass(context->instance);
  // 取得mainActivity放置java函類的類別
  jmethodID updateUI = env->GetMethodID(cls, "updateUI", "()V");
  // 呼叫java函式
  env->CallVoidMethod(context->instance, updateUI);
  // 釋放context記憶體
  delete(context);
  context = nullptr;
  // 把env從多執行緒分離
  _vm->DetachCurrentThread();
  return 0;
}
extern "C"
JNIEXPORT void JNICALL
Java_com_example_ndkproj_MainActivity_testThread(JNIEnv *env, jobject thiz) {
  // pthread_t是long，以下可作為long pid
  pthread_t pid;
  // 建立自定義context物件
  Context *context = new Context;
  // thiz是mainActivity放置java函式的類別
  // 建立全域參考
  context->instance = env->NewGlobalRef(thiz);
  // 啟動執行緒
  // 參數1: pid
  // 參數2: 0
  // 參數3: 執行緒呼叫的函式
  // 參數4: 給函式傳參數，參數是context
  pthread_create(&pid, 0 ,threadTask, context);
}
{% endhighlight %}

lsn7_jni資料
jni+Specification.CHM

