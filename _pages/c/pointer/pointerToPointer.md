---
title: 指標的指標
date: 2024-06-05
keywords: c++, pointer to pointer
---

指標的指標，意思是指標的位址，指標也是位址，所以可以稱為，位址的位址。
### 指標的位址
{% highlight c++ linenos %}
int main() {
    int i = 40;
    cout << "i的值 = " << i << "，i的位址 = " << &i << endl;
    int *p = &i;
    cout << "指標p的值 = " << p << "，指標p的位址 = " << &p << "，對指標p儲存的位址取出存放的內容=" << *p << endl;
    int **pp = &p;
    cout << "指標pp的值 = " << pp << "，指標pp的位址 = " << &pp << "，對指標pp儲存的位址取出存放的內容=" << *pp << endl;
    cout << "再次把上一個步驟取出的內容為位址，再對這個位址取出存放內容=" << **pp << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
i的值 = 40，i的位址 = 0x7ff7bfeff468
指標p的值 = 0x7ff7bfeff468，指標p的位址 = 0x7ff7bfeff460，對指標p儲存的位址取出存放的內容=40
指標pp的值 = 0x7ff7bfeff460，指標pp的位址 = 0x7ff7bfeff458，對指標pp儲存的位址取出存放的內容=0x7ff7bfeff468
再次把上一個步驟取出的位址，再對這個位址取出存放內容=40
```

其它程式碼解釋

{% highlight c++ linenos %}
int main() {
    int i4 = 40;
    //宣告一個指標存i4的位址
    int *p2 = &i4;
    //將p2指標的位址傳給pp2
    int **pp2 = &p2;

    //1.把pp2存放的位址，使用取值運算子*，也就是指標p2的位址
    //*pp2;
    //2.再把p2位址，使用取值運算子*，也就是40
    //**pp2
    printf("解出pp2的值:%d\n",**pp2);    
    return 0;
}
{% endhighlight %}

```
執行結果
解出pp2的值:40
```

### 函式參數為指標的指標
設計一個函式，修改指標存放的位址。
若要對指標存放的位址進行變動，函式參數就要使用二個星號\*\* 的指標，接收指標的位址。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void initAddress(int** p){
    cout << "p = " << p << endl;
    *p = new int(10);
    cout  << "p = " << p << ",*p = " << *p << endl;
}
int main() {
    int* p = nullptr;
    initAddress(&p);
    cout << "指標的位址 = " << &p << ",指標存放的位址 = " << p << "，對存放的位址取出內容 = " << *p << endl;
    return 0;
}
{% endhighlight %}

第9行，宣告指標p，初始化為nullptr，也就是沒有指向任何位址。

第10行，呼叫函式initAddress，引數為指標p的位址。

第3行，宣告一個函式initAddress()，參數為指標的位址，所以使用二個星號**宣告。

第4行，印出指標的位址。

第5行，動態配置記憶體位址，位址存放的內容為10，使用new會返回動態配置記憶體的位址。使用`取值運算子*指標名 = 新位址`修改指標存放的位址。

第6行，印出指標p的位址，印出指標p存放的位址。

第11行，印出指標p的位址，印出指標p存放的位址，對存放的位址取出內容。

```
執行結果
p = 0x7ff7bfeff460
p = 0x7ff7bfeff460,*p = 0x600000008050
指標的位址 = 0x7ff7bfeff460,指標存放的位址 = 0x600000008050，對存放的位址取出內容 = 10
```
