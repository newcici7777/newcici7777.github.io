---
title: void*任何資料型態的指標
date: 2024-05-30
keywords: c++, pointer, void*, void
---

## (void*)印出16進制的位址

使用(void*)就可以印出16進制的位址

{% highlight c++ linenos %}
int main() {
    char c = 'a';
    cout << "變數c位址 = " << (void*)&c << endl;
    return 0;
}
{% endhighlight %}
```
變數c位址 = 0x7ff7bfeff46b
```

## 函式的參數為void*指標(位址)

函式的參數為void*指標，表示任何資料型態的指標(位址)都可以傳進函式，而且不需要轉型。

{% highlight c++ linenos %}
//宣告printAddr的函式，參數資料型態為void*指標
void printAddr(void* p) {
    //印出位址
    cout << p << endl;
}
int main() {
    int i = 10;
    //將整數i變數的位址傳入
    printAddr(&i);
    char c = 'a';
    //將字元c變數的位址傳入
    printAddr(&c);
    double d = 150.222;
    //將浮點數d變數的位址傳入
    printAddr(&d);
    return 0;
}
{% endhighlight %}

```
執行結果
0x7ff7bfeff468
0x7ff7bfeff467
0x7ff7bfeff458
```

## 函式傳回值為void*指標

表示可以回傳任何資料型態的指標(位址)，可以轉型成任何資料型態指標。

以下為malloc的回傳資料型態void*指標(位址)，void*指標可以轉成任何資料型態指標，參數size_t  __size代表設定空間大小。

{% highlight c++ linenos %}
void* malloc(size_t __size)
{% endhighlight %}

使用方式

在heap區段，建立10 byte的記憶體空間，將回傳的位址轉成char資料型態的指標。

{% highlight c++ linenos %}
char *name = (char *)malloc(10);//10byte
{% endhighlight %}


在heap區段，建立1mb的記憶體空間，將回傳的位址轉成int資料型態的指標。

{% highlight c++ linenos %}
int *num = (int *)malloc(1 * 1024 * 1024);//1byte*1024 = 1kb ->1kb*1024=1mb
{% endhighlight %}


## 不能對`void*指標使用取值運算子*`

不能對void*指標使用`取值運算子*`，需要轉換成其它資料型態的指標才可以使用`取值運算子*`

{% highlight c++ linenos %}
void printAddr(void* p) {
    //編譯失敗，不能對p指標使用取值運算子*，因為它是void*指標資料型態，必須轉型後才能對指標取出內容
    cout << *p << endl;
}
{% endhighlight %}


{% highlight c++ linenos %}
void printAddr(void* p) {
    //先將p指標轉型成char*指標，接著使用`取值運算子*`取出指標位址中的內容
    cout << *(char*)p << endl;
}
int main() {
    char c = 'a';
    printAddr(&c);
    return 0;
}
{% endhighlight %}

```
執行結果
a
```

## 函式的參數為void

函式的參數為void，代表不接受任何參數。

rand()產生亂數，以下為rand函式的定義，不接受任何參數，回傳int

{% highlight c++ linenos %}
int rand (void);
{% endhighlight %}

產生0-100的亂數
{% highlight c++ linenos %}
    int num = rand()%100;
    printf("rand = %d \n", num);
{% endhighlight %}