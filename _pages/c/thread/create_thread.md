---
title: 建立thread
date: 2024-10-21
keywords: c++, thread
---

## thread建構子介紹

### 匯入

```
#include <thread>
```

### 建立thread
```
thread t1(func, "test test");
```
- 第一個參數是函式名
- 第二個參數是呼叫函式代入的參數

### sleep函式

使用sleep函式，需要匯入以下標頭檔，因為我是mac，所以匯入unistd.h

```
#include <unistd.h>
```

sleep函式使用方式
```
sleep(1);
```
在mac中，sleep是小寫，建構子代入參數1，代表1秒。

### join 執行緒記憶體釋放

{% highlight c++ linenos %}
  t1.join();
{% endhighlight %}

### 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <thread>
using namespace std;
void func(string msg) {
  for (int i = 0; i <= 10; i++) {
    cout << "i = " << i << ", msg =" << msg << endl;
    sleep(1);//停1秒鐘
  }
}
int main() {
  //建立執行緒t1
  thread t1(func, "test test");
  //建立執行緒t2
  thread t2(func, "abcdefg abcdefg");
  
  //執行緒t1被記憶體釋放
  t1.join();
  //執行緒t2被記憶體釋放
  t2.join();
  return 0;
}
{% endhighlight %}
```
i = 0, msg =test test
i = 0, msg =abcdefg abcdefg
i = 1, msg =test test
i = 1, msg =abcdefg abcdefg
i = 2, msg =abcdefg abcdefg
i = 2, msg =test test
i = 3, msg =abcdefg abcdefg
i = 3, msg =test test
i = 4, msg =abcdefg abcdefg
i = 4, msg =test test
i = 5, msg =abcdefg abcdefg
i = 5, msg =test test
i = 6, msg =i = 6, msg =test test
abcdefg abcdefg
i = 7, msg =test test
i = 7, msg =abcdefg abcdefg
i = 8, msg =test test
i = 8, msg =abcdefg abcdefg
i = 9, msg =test test
i = 9, msg =abcdefg abcdefg
i = 10, msg =abcdefg abcdefg
i = 10, msg =test test
```

## thread與成員函式

```
thread 執行緒變數(&類別名::成員函式, &物件名, 傳進成員函式的參數1,傳進成員函式的參數2);
```
- 參數1為成員函式的記憶體位址
- 參數2為物件記憶體位址
- 參數3與參數4為函式的參數

以下的程式碼包含Student的類別，t3執行緒是呼叫Student物件的成員函式func()，並代入參數msg。

{% highlight c++ linenos %}
  Student student;//建立物件
  //第一個參數傳入成員函式位址，要有&類別名::成員函式，注意！成員函式結尾不用括號
  //第二個參數傳入物件地址
  //第三個參數傳入函式的參數
  thread t3(&Student::func, &student, "ccccccc");
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
class Student {
  public :
  void func(const string& msg) {
    for (int i = 0; i <= 10; i++) {
      cout << " i = " << i << ", msg =" << msg << endl;
    }
  }
};
int main() {
  //建立執行緒t1
  thread t1(func, "test test");
  //建立執行緒t2
  thread t2(func, "abcdefg abcdefg");
  
  Student student;//建立物件
  //第一個參數傳入成員函式位址，要有&類別名::成員函式，注意！成員函式結尾不用括號
  //第二個參數傳入物件地址
  //第三個參數傳入函式的參數
  thread t3(&Student::func, &student, "ccccccc");
  
  //執行緒t1被記憶體釋放
  t1.join();
  //執行緒t2被記憶體釋放
  t2.join();
  t3.join();
  return 0;
}
{% endhighlight %}