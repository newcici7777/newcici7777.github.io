---
title: Math
date: 2025-06-11
keywords: java, Math
---
Math有很多數學方法，以下介紹常用。

## abs絕對值
{% highlight java linenos %}
System.out.println(Math.abs(-9));
{% endhighlight %}
9

## pow次方
2的4次方。
{% highlight java linenos %}
System.out.println(Math.pow(2, 4));
{% endhighlight %}
16

## ceil
傳回大於或等於參數的「整數」，不是小數。

以下程式碼傳回 \>= 3.99的「整數」，ceil回傳值是double。
{% highlight java linenos %}
System.out.println(Math.ceil(3.99));
{% endhighlight %}
```
4.0
```

## floor
傳回小於或等於參數的「整數」，不是小數。

以下程式碼傳回 \<= 3.99的「整數」，floor回傳值是double。
{% highlight java linenos %}
System.out.println(Math.floor(3.99));
{% endhighlight %}
```
3.0
```

## round四捨五入
{% highlight java linenos %}
System.out.println(Math.round(3.99));
System.out.println(Math.round(3.45));
{% endhighlight %}
```
4
3
```

強制轉型成int是無條件捨去小數點。
{% highlight java linenos %}
System.out.println((int) 3.99);
System.out.println((int) 3.45);
{% endhighlight %}
```
3
3
```

## sqrt開根號
3 \* 3 = 9，9的開根號是3。
{% highlight java linenos %}
System.out.println(Math.sqrt(9));
{% endhighlight %}
```
3
```

## min 二個數字取最小值
二個數字可以放int, float, double
{% highlight java linenos %}
System.out.println(Math.min(3.5, 5.6));
{% endhighlight %}
```
3.5
```

## max 二個數字取最大值
{% highlight java linenos %}
System.out.println(Math.max(3.5, 5.6));
{% endhighlight %}
```
5.6
```