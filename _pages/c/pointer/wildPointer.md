---
title: 指標地址是無效內容
date: 2024-05-30
keywords: c++, pointer, wildPointer
---

## 釋放記憶體後的指標

釋放指標記憶體地址中的內容，但指標仍指在原本的地址上。

{% highlight c++ linenos %}
int main() {
    int* p = new int(10);
    cout << "記憶體釋放前的地址 = " << p << endl;
    delete p;
    cout << "記憶體釋放後的地址 = " << p << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
記憶體釋放前的地址 = 0x600000004060
記憶體釋放後的地址 = 0x600000004060
```

如果對已經被釋放(清空)的地址進行讀取，讀取的內容不知道是什麼。

{% highlight c++ linenos %}
int main() {
    int* p = new int(10);
    cout << "記憶體釋放前的地址 = " << p << endl;
    cout << "記憶體釋放前的地址讀取地址內容 = " << *p << endl;
    delete p;
    cout << "記憶體釋放後的地址 = " << p << endl;
    cout << "讀取地址內容 = " << *p << endl;
    return 0;
}
{% endhighlight %}
第7行，讀取已釋放記憶體的地址中的內容。


```
執行結果
記憶體釋放前的地址 = 0x60000000c010
記憶體釋放前的地址讀取地址內容 = 10
記憶體釋放後的地址 = 0x60000000c010
讀取地址內容 = -559038448
```

為避免上面的情況，釋放完記憶體，要把記憶體地址設為nullptr

{% highlight c++ linenos %}
int main() {
    int* p = new int(10);
    cout << "記憶體釋放前的地址 = " << p << endl;
    cout << "記憶體釋放前的地址讀取地址內容 = " << *p << endl;
    delete p;
    p = nullptr;
    cout << "記憶體釋放後的地址 = " << p << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
記憶體釋放前的地址 = 0x60000000c010
記憶體釋放前的地址讀取地址內容 = 10
記憶體釋放後的地址 = 0x0
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

第3行,印出地址，印出不知道何處的地址。

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

第3行,印出地址，印出0。

```
執行結果
0x0
```

## 函式返回區域變數地址

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

第3行,印出區域變數i的值與地址。

第4行,返回變數i的地址。

第7行,宣告指標p，取得函式返回的地址。

第8行,印出函式返回的地址中的值，與地址。

```
執行結果
i = 10, &i = 0x7ff7bfeff44c
*p = 32760, p = 0x7ff7bfeff44c
```

由以上執行結果可以發現區域變數i的地址與返回的地址不同，而且值也不一樣。