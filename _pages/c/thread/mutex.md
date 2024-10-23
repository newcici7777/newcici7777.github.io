---
title: 互斥鎖Mutex
date: 2024-10-22
keywords: c++, mutex
---

## 執行緒安全

多個執行緒搶同一個資源，就會發生問題，以下程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <thread>
using namespace std;
void func(string msg) {
    //每一秒印1次，這裡是執行10秒
    for(int i = 0; i < 10; i++) {
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
i = 3i = 3, msg =test test, msg =abcdefg abcdefg

i = 4i = 4, msg =abcdefg abcdefg, msg =test test

i = 5, msg =test test
i = 5, msg =abcdefg abcdefg
i = 6, msg =test test
i = 6, msg =abcdefg abcdefg
i = 7, msg =test test
i = 7, msg =abcdefg abcdefg
i = 8, msg =abcdefg abcdefg
i = 8, msg =test test
i = 9, msg =abcdefg abcdefg
i = 9, msg =test test
```

從執行結果來看，以下二句是二個執行緒搶cout(全域變數)這個資源，導致輸出的結果不正常。
```
i = 3i = 3, msg =test test, msg =abcdefg abcdefg

i = 4i = 4, msg =abcdefg abcdefg, msg =test test
```

## 互斥鎖Mutex

同步(Concurrency Control)是指，多個執行緒協調如何使用同一個資源。

如下圖，每一個人代表一個執行緒，他們都在排隊上同一間廁所(同一個資源)，當一個人(一個執行緒)進入廁所，就要把門鎖上，當上完廁所，再把門打開，輪下一個人(下一個執行緒)進去上廁所(同一個資源)。

![img]({{site.imgurl}}/toilet.png)  

互斥鎖保證同一個時間只有一個人(一個執行緒)使用廁所(同一個資源)，當一個執行緒把門鎖上，其它執行緒就在門外排隊等候，等待廁所中的執行緒把門打開(開鎖)，輪下一個執行緒進去把門鎖上。

### 匯入標頭檔

```
#include <mutex>
```

### 互斥鎖全域變數

```
mutex mtx;
```

### lock() 鎖上

可以把mutex想像成是廁所那個門，若是門是鎖起來的狀態，代表有人(一個thread)在廁所裡面，其它人(threads)就要在門外排隊等待，直到門開鎖，沒有鎖起來的狀態，執行緒呼叫mutex.lock()會自行判斷鎖起來的狀態，若沒鎖上，執行緒會得到鎖，若鎖上，執行緒就要排隊等待。

### unlock() 開鎖

只有在廁所中的執行緒才有開鎖的功能，其它在廁所門外排隊的執行緒是沒辦法開鎖。


### 程式碼

先前的程式，把cout想成廁所，程式碼修改如下

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <thread>
#include <mutex>
using namespace std;
//建立mutex互斥鎖
mutex mtx;
void func(string msg) {
    //每一秒印1次，這裡是執行10秒
    for(int i = 0; i < 10; i++) {
        mtx.lock();//把門鎖上
        cout << "i = " << i << ", msg =" << msg << endl;
        sleep(1);//持有鎖後，再休眠1秒
        mtx.unlock();//使用完畢，把門開鎖
    }
}
{% endhighlight %}

```
i = 0, msg =test test
i = 1, msg =test test
i = 2, msg =test test
i = 3, msg =test test
i = 4, msg =test test
i = 5, msg =test test
i = 0, msg =abcdefg abcdefg
i = 1, msg =abcdefg abcdefg
i = 2, msg =abcdefg abcdefg
i = 3, msg =abcdefg abcdefg
i = 4, msg =abcdefg abcdefg
i = 5, msg =abcdefg abcdefg
i = 6, msg =abcdefg abcdefg
i = 7, msg =abcdefg abcdefg
i = 8, msg =abcdefg abcdefg
i = 9, msg =abcdefg abcdefg
i = 6, msg =test test
i = 7, msg =test test
i = 8, msg =test test
i = 9, msg =test test
```

### 判斷是那個thread加鎖/解鎖/申請鎖

{% highlight c++ linenos %}
void func(string msg) {
    //每一秒印1次，這裡是執行10秒
    for(int i = 0; i < 10; i++) {
        cout << "申請鎖的thread = " << this_thread::get_id() << endl;
        mtx.lock();
        cout << "鎖上的thread = " << this_thread::get_id() << endl;
        cout << "i = " << i << ", msg =" << msg << endl;
        sleep(1);//持有鎖後，再休眠1秒
        cout << "開鎖的thread = " << this_thread::get_id() << endl;
        mtx.unlock();
    }
}
{% endhighlight %}

### 類別與mutex

{% highlight c++ linenos %}
class Student {
    //私有成員變數互斥鎖
    mutex m_mutex;
public:
    void func(string msg) {
        for(int i = 0; i < 10; i++) {
            m_mutex.lock();
            cout << "msg = " << msg << endl;
            m_mutex.unlock();
        }
    }
};
int main() {
    Student student;//建立物件
    thread t1(&Student::func, &student, "student1");
    thread t2(&Student::func, &student, "student2");
    thread t3(&Student::func, &student, "student3");
    thread t4(&Student::func, &student, "student4");
    thread t5(&Student::func, &student, "student5");
    t1.join();
    t2.join();
    t3.join();
    t4.join();
    t5.join();
    return 0;
}
{% endhighlight %}

```
msg = student1
msg = student3
msg = student5
msg = student5
msg = student5
msg = student5
msg = student5
msg = student5
msg = student5
msg = student5
msg = student5
msg = student5
以下截斷
```

## recursive_mutex

以下程式碼，func4()呼叫func3()，但執行結果只印出func4。
若是鎖中有鎖，會有死鎖(Dead lock)問題，mutex必須解鎖才能加鎖。

{% highlight c++ linenos %}
mutex mtx;
void func3() {
    mtx.lock();//加鎖
    cout << "func3" << endl;
    mtx.unlock();//解鎖
}
void func4() {
    mtx.lock();//加鎖
    cout << "func4" << endl;
    func3();//呼叫func3
    mtx.unlock();//解鎖
}
int main() {
    //建立執行緒t1
    thread t1(func4, "test test");
    //執行緒t1被記憶體釋放
    t1.join();
    return 0;
}
{% endhighlight %}
```
func4
```

改成使用recursive_mutex，就不會有死鎖問題。

{% highlight c++ linenos %}
recursive_mutex r_mtx;
void func3() {
    r_mtx.lock();//加鎖
    cout << "func3" << endl;
    r_mtx.unlock();//解鎖
}
void func4() {
    r_mtx.lock();//加鎖
    cout << "func4" << endl;
    func3();//呼叫func3
    r_mtx.unlock();//解鎖
}
int main() {
    //建立執行緒t1
    thread t1(func4);
    //執行緒t1被記憶體釋放
    t1.join();
    return 0;
}
{% endhighlight %}
```
func4
func3
```

## lock_guard

lock_guard是mutex的樣板，只要管加鎖，不用管解鎖，它會在離開有效範圍(Scope)與生命週期(Lifetime)，自動解鎖。

語法
{% highlight c++ linenos %}
lock_guard<mutex類別名> mlock(mtx物件名);
{% endhighlight %}

把之前的程式碼修改成lock_guard

{% highlight c++ linenos %}
void func2(string msg) {
    //每一秒印1次，這裡是執行10秒
    for(int i = 0; i < 10; i++) {
        cout << "申請鎖的thread = " << this_thread::get_id() << endl;
        //有效範圍scope 開始
        {
            lock_guard<mutex> mlock(mtx);//把門鎖上
            cout << "鎖上的thread = " << this_thread::get_id() << endl;
            cout << "i = " << i << ", msg =" << msg << endl;
            sleep(1);//持有鎖後，再休眠1秒
            cout << "開鎖的thread = " << this_thread::get_id() << endl;
        }
        //有效範圍scope 結束
    }
}
int main() {
    //建立執行緒t1
    thread t1(func2, "test test");
    //建立執行緒t2
    thread t2(func2, "abcdefg abcdefg");
    //執行緒t1被記憶體釋放
    t1.join();
    //執行緒t2被記憶體釋放
    t2.join();
    return 0;
}
{% endhighlight %}

```
申請鎖的thread = 0x700005e2a000
鎖上的thread = 0x700005e2a000
i = 0, msg =test test
申請鎖的thread = 0x700005ead000
開鎖的thread = 0x700005e2a000
申請鎖的thread = 0x700005e2a000
鎖上的thread = 0x700005e2a000
i = 1, msg =test test
開鎖的thread = 0x700005e2a000
申請鎖的thread = 0x700005e2a000
鎖上的thread = 0x700005e2a000
i = 2, msg =test test
開鎖的thread = 0x700005e2a000
申請鎖的thread = 0x700005e2a000
鎖上的thread = 0x700005e2a000
i = 3, msg =test test
開鎖的thread = 0x700005e2a000
申請鎖的thread = 0x700005e2a000
鎖上的thread = 0x700005ead000
i = 0, msg =abcdefg abcdefg
開鎖的thread = 0x700005ead000
申請鎖的thread = 0x700005ead000
鎖上的thread = 0x700005ead000
i = 1, msg =abcdefg abcdefg
開鎖的thread = 0x700005ead000
申請鎖的thread = 0x700005ead000
鎖上的thread = 0x700005ead000
i = 2, msg =abcdefg abcdefg
開鎖的thread = 0x700005ead000
申請鎖的thread = 0x700005ead000
鎖上的thread = 0x700005ead000
i = 3, msg =abcdefg abcdefg
開鎖的thread = 0x700005ead000
申請鎖的thread = 0x700005ead000
鎖上的thread = 0x700005ead000
i = 4, msg =abcdefg abcdefg
開鎖的thread = 0x700005ead000
以下截斷
```