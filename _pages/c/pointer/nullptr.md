---
title: 0 == nullptr == NULL
date: 2024-05-30
keywords: c++, nullptr
---

## nullptr代表沒有指向任何位址

如果對沒有指向任何位址的指標，也就是指標 = nullptr，對這個指標使用`*取值運算子`，試圖取出位址中的內容，編譯可以通過，但執行時將會出現錯誤。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func1(int* param1) {
    cout << "param1=" << *param1 << endl;
}
int main() {
    int* p = nullptr;
    func1(p);
    return 0;
}
{% endhighlight %}

第7行，宣告指標p，沒有指向任何位址。

第8行，將指標p傳入函式。

第4行，將指標p使用`*取值運算子`試圖取出位址中的內容，但由於指標沒有指向任何位址，執行時會產生錯誤。


### 檢查nullptr

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func1(int* param1) {
    if(param1 == nullptr) return;
    cout << "param1=" << *param1 << endl;
}
int main() {
    int* p = nullptr;
    func1(p);
    return 0;
}
{% endhighlight %}

第4行，判斷指標是不是沒有指向任何位址，若沒有指向任何位址，函式直接返回，不再往下執行。

### delete nullptr

delete nullptr 不會有編譯錯誤與執行錯誤，以下程式不會有任何錯誤。

{% highlight c++ linenos %}
int main() {
    int* p = nullptr;
    delete p;
    return 0;
}
{% endhighlight %}

## NULL

在c的標準函式庫中stddef.h定義了常數NULL就是0與nullptr。

{% highlight c++ linenos %}
#define NULL 0
//since C++11
#define NULL nullptr
{% endhighlight %}
第1行定義常數NULL的值為0，代表沒有指向任何記憶空間

第3行，c++11以後，定義常數NULL為nullptr，代表沒有指向任何記憶空間

## 0

0，代表沒有指向任何記憶空間

以下為同名函式，但參數不同，一個為int資料型態，一個為int指標，呼叫函式(0)，呼叫的是參數為整數資料型態的函式。

{% highlight c++ linenos %}
void func3(int n) {
    printf("n = %d\n",n);//印出值
}
void func3(int* p) {
    printf("位址 = %#x\n",p);//印出位址
}
int main() {
    func3(0);
    return 0;
}
{% endhighlight %}

```
執行結果
n = 0
```

## 轉型

把指標參數設為NULL，呼叫參數為指標的函式，以下這樣寫會出錯。

{% highlight c++ linenos %}
func3(NULL);//指標參數設為NULL
{% endhighlight %}

因為編譯不知道你的NULL是整數指標資料型態的NULL，轉型就可以編譯成功，以下三行都可以。

{% highlight c++ linenos %}
func3(nullptr);
func3((int *)nullptr);
func3(static_cast<int *>(nullptr));
{% endhighlight %}

第1行，nullptr代表所有資料型態的NULL。

第2行，轉型int指標。

第3行，使用static_cast轉型成int指標，與上一行意思相同。


完整程式碼

{% highlight c++ linenos %}
void func3(int n) {
    printf("n = %d\n",n);//印出值
}
void func3(int* p) {
    printf("位址 = %#x\n",p);//印出位址
}
int main() {
    func3(0);
    func3((int *)NULL);
    func3(static_cast<int *>(NULL));
    func3(nullptr);
    return 0;
}
{% endhighlight %}

```
執行結果
n = 0
位址 = 0
位址 = 0
位址 = 0
```

## nullptr位址為0

以下程式碼印出nullptr指標的位址，會印出0。
{% highlight c++ linenos %}
int main() {
    int* p = nullptr;
    cout << p << endl;
    delete p;
    return 0;
}
{% endhighlight %}

```
執行結果
0x0
```

## nullptr記憶體位址

0x00000000-0x0000FFFF記憶體位址區間是放置nullptr指標，無法讀取這段記憶體位址區間。

原文
>Each process' virtual address space is split into partitions. On x86 32-Bit Windows, the partition of 0x00000000 - 0x0000FFFF (inclusive) is called NULL-Pointer Assignment Partition. This partition is set aside to help programmers catch NULL-pointer assignments. If a thread in your aprocess attempts to read from or write to a memory address in this partition, an access violoation is raised.

## 容易混淆寫法

{% highlight c++ linenos %}
    int a = 0 ;
    int * p1 = 0 ; //right
    int * p2 = NULL ; //right
    int * p3 = a;//error
{% endhighlight %}

第1行定義整數資料型態的a變數，值為0

第2行定義p1指標沒有指向任何記憶體空間

第3行定義p2指標沒有指向任何記憶體空間

第4行錯誤，指標是存放位址，而不是值，不能把整數值0放進指標
