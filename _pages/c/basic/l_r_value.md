---
title: lvalue與rvalue
date: 2024-06-22
keywords: c++, lvalue
---

```
lvalue = rvalue;
```

在等號左邊的叫lvalue，在等號右邊的叫rvalue。

## lvalue

以下都是lvalue:

### 有定義資料型態(int, double, float, char, long long ...)的變數，可以指派值。

{% highlight c++ linenos %}
    // i is lvalue
    int i;
    //j is lvalue
    int j = 10; 	// 10 is rvalue
{% endhighlight %}

### 有定義資料型態(int, double, float, char, long long ...)的指標，可以指向記憶體位址。

{% highlight c++ linenos %}
    //p1 is lvalue
    int* p1;
    //p2 is lvalue
    int* p2 = &j;	//&j is rvalue
{% endhighlight %}

### 可以用\*取值運算子修改指標指向的記憶體位址的值。

{% highlight c++ linenos %}
    //j is lvalue
    int j = 10; //  10 is rvalue
    
    //p2 is lvalue
    int* p2 = &j;   //&j is rvalue
    
    //*p2 is lavlue
    *p2 = 100;  //100 is rvalue
{% endhighlight %}


等號左邊(lavlue)，也就是一個能夠擺在等號左邊的東西∶**一個變數，而非常數**。

等號左邊可以是變數、陣列元素、結構、[參考]({% link _pages/c/reference/reference.md %})、[取值運算子]({% link _pages/c/pointer/pointer.md %}#取值運算子)。

常數可以是[字串常數]({% link _pages/c/array/pointerCharArr.md %}#字串常數)、表達式。
