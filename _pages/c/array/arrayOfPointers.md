---
title: 指標陣列存放多個記憶體位址
date: 2024-06-19
keywords: c++, point of arrays
---

## 指標陣列

{% highlight c++ linenos %}
    int i1 = 10;
    int i2 = 20;
    int i3 = 30;
    //宣告指標陣列存放3個記憶體位址
    int *p_array[3] = {&i1, &i2, &i3};
{% endhighlight %}

## 取址與取值

方式有二種
- 指標方式

使用指標+i迴圈印出，用取值運算子`*(p_array + i)`取出記憶體位址，再把取出的記憶體位址再次使用\*取值運算子，取出記憶體位址存放的內容。

{% highlight c++ linenos %}
for (int i = 0; i < 3; ++i) {
    printf("i= %d的位置%#x, 值為%d\n",i, *(p_array + i), *(*(p_array + i)));
}
{% endhighlight %}

```
i= 0的位置0xbfeff3fc, 值為100
i= 1的位置0xbfeff3cc, 值為20
i= 2的位置0xbfeff3c8, 值為30
```

- 陣列方式

使用[索引]取出陣列存放的記憶體位址，再對記憶體位址使用取值運算子\*，取出記憶體位址存放的內容。

{% highlight c++ linenos %}
for (int i = 0; i < 3; ++i) {
	printf("i=%d的位置%#x, 值為%d\n",i, p_array[i], *p_array[i]);
}
{% endhighlight %}

## 指標陣列存放多個字串常數位址

參考

[指標陣列存放多個字串常數位址]({% link _pages/c/array/pointerCharArr.md %}#指標陣列存放多個字串常數位址)