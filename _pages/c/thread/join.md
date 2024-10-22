---
title: 執行緒記憶體釋放
date: 2024-10-21
keywords: c++, join
---

## main()函式執行完成，執行中的執行緒強迫結束

- 執行緒函式返回，執行緒結束。
- main程式退出，正在執行的執行緒都會被強迫結束。

以下範例，執行緒執行10秒結束，但main函式(主程式)
只執行5秒結束，明顯執行緒執行的時間比主程式久，重點是主程式結束，其它未執行完的執行緒也會強迫結束。

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
    
    //每一秒印1次，這裡是執行5秒
    for(int i = 0; i > 5; i++) {
        cout << "i = " << i << "秒" << endl;
        sleep(1);
    }
    return 0;
}
{% endhighlight %}

## 釋放執行緒記憶體位址

多個執行緒共享Stack空間，每一個執行緒都會占記憶體空間，所以執行緒結束時要釋放它的記憶體位址。

執行緒記憶體釋放有2種方法

- 在main()函式使用join()，等待執行緒結束，記憶體位址釋放(也稱記憶體回收)後，返回main()主程式。
- 使用detach()執行緒與主程式分離，各別運行，執行緒結束後會自行使用joinalbe()把它占用的記憶體位址釋放。

即便以下程式，main()主程式執行時間改為12秒，比執行緒10秒久，但執行緒記憶體位址沒被釋放，仍會執行錯誤。

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
    
    //每一秒印1次，這裡是執行12秒
    for(int i = 0; i > 12; i++) {
        cout << "i = " << i << "秒" << endl;
        sleep(1);
    }
    
    return 0;
}
{% endhighlight %}

### join()

以下程式碼，即便main()函式已經刪掉執行時間12秒的程式碼，也就是說main()函式的執行時間為0秒，但使用join()，main()函式仍會等待執行緒都執行完(包含記憶體釋放)，才會跟著結束。

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

### detach()

使用detach()分離執行緒，main()函式要執行的比執行緒久，不然的話，主程式執行結束，執行緒也會跟著執行結束。

{% highlight c++ linenos %}
int main() {
    //建立執行緒t1
    thread t1(func, "test test");
    //建立執行緒t2
    thread t2(func, "abcdefg abcdefg");
    //分離執行緒
    t1.detach();
    //分離執行緒
    t2.detach();
    //main()函式等留12秒，等待執行緒(執行緒10秒)
    sleep(12);
    return 0;
}
{% endhighlight %}
