---
title: 指標位址是無效內容
date: 2024-05-30
keywords: c++, pointer, wildPointer
---

## 回收記憶體後的指標

回收指標記憶體位址中的內容，但指標仍指在原本的位址上。

{% highlight c++ linenos %}
int main() {
    int* p = new int(10);
    cout << "記憶體回收前的位址 = " << p << endl;
    delete p;
    cout << "記憶體回收後的位址 = " << p << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
記憶體回收前的位址 = 0x600000004060
記憶體回收後的位址 = 0x600000004060
```

如果對已經被回收(清空)的位址進行讀取，讀取的內容不知道是什麼。

{% highlight c++ linenos %}
int main() {
    int* p = new int(10);
    cout << "記憶體回收前的位址 = " << p << endl;
    cout << "記憶體回收前的位址讀取位址內容 = " << *p << endl;
    delete p;
    cout << "記憶體回收後的位址 = " << p << endl;
    cout << "讀取位址內容 = " << *p << endl;
    return 0;
}
{% endhighlight %}
第7行，讀取已回收記憶體的位址中的內容。


```
執行結果
記憶體回收前的位址 = 0x60000000c010
記憶體回收前的位址讀取位址內容 = 10
記憶體回收後的位址 = 0x60000000c010
讀取位址內容 = -559038448
```

為避免上面的情況，回收完記憶體，要把記憶體位址設為nullptr

{% highlight c++ linenos %}
int main() {
    int* p = new int(10);
    cout << "記憶體回收前的位址 = " << p << endl;
    cout << "記憶體回收前的位址讀取位址內容 = " << *p << endl;
    delete p;
    p = nullptr;
    cout << "記憶體回收後的位址 = " << p << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
記憶體回收前的位址 = 0x60000000c010
記憶體回收前的位址讀取位址內容 = 10
記憶體回收後的位址 = 0x0
```

## 未初始化的指標

{% highlight c++ linenos %}
int main() {
    int* p;
    cout << p << endl;
    return 0;
}
{% endhighlight %}

第2行,宣告未初始化的指標。

第3行,印出位址，印出不知道何處的位址。

```
執行結果
0x1e
```

建議把宣告指標要設初始值。

{% highlight c++ linenos %}
int main() {
    int* p = nullptr;
    cout << p << endl;
    return 0;
}
{% endhighlight %}

第2行,宣告指標，初始化nullptr。

第3行,印出位址，印出0。

```
執行結果
0x0
```

## 函式返回區域變數位址

{% highlight c++ linenos %}
int* func() {
    int i = 10;
    cout << "i = " << i << ", &i = " << &i << endl;
    return &i;
}
int main() {
    int* p = func();
    cout << "*p = " << *p << ", p = " << p << endl;
    return 0;
}
{% endhighlight %}

第1行,宣告函式，返回指標。

第3行,印出區域變數i的值與位址。

第4行,返回變數i的位址。

第7行,宣告指標p，取得函式返回的位址。

第8行,印出函式返回的位址中的值，與位址。

```
執行結果
i = 10, &i = 0x7ff7bfeff44c
*p = 32760, p = 0x7ff7bfeff44c
```

由以上執行結果可以發現區域變數i的位址與返回的位址不同，而且值也不一樣。