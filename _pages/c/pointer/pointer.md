---
title: 指標基本觀念
date: 2024-05-27
keywords: c++, pointer
---

## 變數地址

每個變數系統會分配一個記憶體地址存放變數，地址通常是用16進制表示，但為了方便識別，可將地址轉成10進制。

## 取地址運算子&

`&變數`

在變數前面加上&可以取出變數的記憶體地址。

{% highlight c++ linenos %}
int main() {
    int a = 0;
    double b = 10.5;
    cout << "變數a地址 = " << &a << endl;
    cout << "變數b地址 = " << &b << endl;
    return 0;
}
{% endhighlight %}
```
執行結果
變數a地址 = 0x7ff7bfeff468
變數b地址 = 0x7ff7bfeff460
```

## 指標是一種變數，存放的內容是地址。

以下為宣告一個i1的整型變數，初始值為10。

{% highlight c++ linenos %}
int i1 = 10;
{% endhighlight %}

指標變數存放的內容是地址，以下為宣告一個p1的指標變數，存放i1的地址。

{% highlight c++ linenos %}
int* p1 = &i1;
{% endhighlight %}

## 指標的大小8byte
指標變數的大小8byte，使用sizeof(指標變數)可以取得指標的大小，不管是什麼類型的指標，佔用記憶體的大小全部為8byte。
{% highlight c++ linenos %}
cout << sizeof(p1) << endl;
{% endhighlight %}
```
執行結果
8
```

## 地址放入指標變數
指標存放的內容是地址，所以要先取得變數的地址，再放入指標變數中，使用&可以取得變數地址。

{% highlight c++ linenos %}
int main() {
    int i1 = 10;
    int* p1 = &i1;
    cout << "i1的地址=" << &i1 << endl;
    cout << "p1存放的地址 =" << p1 << endl;
    return 0;
}
{% endhighlight %}

第2行，宣告i1整型變數。

第3行，宣告p1整型指標變數，使用取地址運算子&，取出i1變數的記憶體地址，將地址放入p1指標。

第4行，印出i1的地址。

第5行，印出p1所存放的內容。


```
執行結果
i1的地址=0x7ff7bfeff468
p1存放的地址 =0x7ff7bfeff468
```

從執行結果可以發現，印出的結果是一樣。


## 宣告指標的各種寫法

宣告指標可以把*放置在`變數前面`或`類型後面`或`類型與變數中間`，以下都是正確的。

{% highlight c++ linenos %}
int main() {
    int i1 = 10;
    int *p1 = &i1;		//*在變數前面
    int* p2 = &i1;		//*在類型後面
    int * p3 = &i1;		//*在類型與變數中間
    cout << "i1的地址=" << &i1 << endl;
    cout << "p1存放的地址 =" << p1 << endl;
    cout << "p2存放的地址 =" << p2 << endl;
    cout << "p3存放的地址 =" << p3 << endl;
    return 0;
}
{% endhighlight %}
```
執行結果
i1的地址=0x7ff7bfeff468
p1存放的地址 =0x7ff7bfeff468
p2存放的地址 =0x7ff7bfeff468
p3存放的地址 =0x7ff7bfeff468
```

## 取值運算子*

取值運算子*是取得地址中存放的值。指標存放地址，指標也就等於地址。

`*指標`

以上語法為，取出地址中存放的值，而指標就是地址。

{% highlight c++ linenos %}
int main() {
    int i1 = 10;
    int *p1 = &i1;        //將i1變數的地址存到p1指標變數
    cout << "取出p1地址的值 = " << *p1 << endl;	//取出地址存放的值。
    return 0;
}
{% endhighlight %}
```
執行結果
取出p1地址的值 = 10
```

## 修改地址存放的值*

`*指標 = 修改的內容`

把*放在指標變數前面，就可以在等於(=)後面放入要修改的內容。

{% highlight c++ linenos %}
int main() {
    int i1 = 10;
    cout << "取出i1的值 = " << i1 << endl;
    int *p1 = &i1;
    cout << "取出p1地址的值 = " << *p1 << endl;
    *p1 = 20;
    cout << "取出p1地址的值 = " << *p1 << endl;
    cout << "取出i1的值 = " << i1 << endl;
    return 0;
}
{% endhighlight %}
第6行，把*放在指標變數p1前面，等於(=)後面放入要修改的內容20。
```
執行結果
取出i1的值 = 10
取出p1地址的值 = 10
取出p1地址的值 = 20
```

## 指標類型要與值一致。

指標是存放地址，不能存放不是地址的值，會編譯錯誤。
{% highlight c++ linenos %}
int main() {
    int i1 = 10;
    int *p1 = i1;
    return 0;
}
{% endhighlight %}

第2行，i1變數是整數10。

第3行，把10塞入指標，因為指標是存放地址，不能放10這個整數，產生編譯錯誤。

## 印出地址

### 十六進制

以下的方式印不出char變數的地址。

{% highlight c++ linenos %}
int main() {
    char c = 'a';
    cout << "變數c地址 = " << &c << endl;
    return 0;
}
{% endhighlight %}
```
執行結果
變數c地址 = a
```

使用(void*)就可以印出16進制的地址

`(void*)指標變數`

{% highlight c++ linenos %}
int main() {
    char c = 'a';
    cout << "變數c地址 = " << (void*)&c << endl;
    return 0;
}
{% endhighlight %}
```
執行結果
變數c地址 = 0x7ff7bfeff46b
```




### 十進制

`(long long)指標變數`

因為int只有4byte，地址轉成int整數會超出範圍，使用long long 8byte，就不會有超出數值範圍的問題。

### c語言

`printf("%#x",指標變數);`

使用%#x就可印出16進制的記憶體地址。

### 程式碼範例如下:

{% highlight c++ linenos %}
int main() {
    int i1 = 10;
    int *p1 = &i1;
    cout << "16進制 &i1 = " << (void*)&i1 << endl;
    cout << "16進制 p1 = " << (void*)p1 << endl;
   
    cout << "10進制 &i1 = " << (long long)&i1 << endl;
    cout << "10進制 p1 = " << (long long)p1 << endl;

    printf("c語言 &i1 = %#x \n",&i1);
    printf("c語言 p1 = %#x \n",p1);
    return 0;
}
{% endhighlight %}
```
執行結果
16進制 &i1 = 0x7ff7bfeff468
16進制 p1 = 0x7ff7bfeff468
10進制 &i1 = 140702053823592
10進制 p1 = 140702053823592
c語言 &i1 = 0xbfeff468 
c語言 p1 = 0xbfeff468
```
