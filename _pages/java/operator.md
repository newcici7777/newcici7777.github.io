---
title: 運算子與運算元
date: 2025-06-25
keywords: Java, operator
---
## 運算子
算術運算子:

\+ \- $ \times $ $ \div $ %


關係運算子:
```
> < >= <= != == 
```

邏輯運算子:
```
&& & || |
```

位元運算子:
```
& AND
| OR
^ XOR
```

負數運算子 \-

0與1位元互換，位元反轉運算子 \~

## 運算元
運算子二邊的數字、字串、變數，都是運算元。

5跟3是運算元。
```
5 + 3
```

i1,i2是運算元。
{% highlight java linenos %}
int i1 = 10;
int i2 = 20;
int i3 = i1 + i2;
{% endhighlight %}

abc與defg是運算元。
{% highlight java linenos %}
String s = "abc" + "defg";
{% endhighlight %}

## 邏輯運算子
```
&& 第1個判斷句為false，不會執行第2個判斷句
&  第1個判斷句為false，會執行第2個判斷句
|| 第1個判斷句為true，不會執行第2個判斷句
|  第1個判斷句為true，會執行第2個判斷句
```

&&與&都是2邊判斷句為true，才是true。

\|\|與\|都是有一個判斷句為true，就是true。

差別在於，會不會執行第2個判斷句。


以下不會執行第2個判斷句。
{% highlight java linenos %}
int age = 50;
if (age < 10 && age++ < 20) {
  System.out.println("判斷1");
}
System.out.println("age = " + age);
{% endhighlight %}
執行結果50

以下「會執行」第2個判斷句。
{% highlight java linenos %}
int age = 50;
if (age < 10 & age++ < 20) {
  System.out.println("判斷1");
}
System.out.println("age = " + age);
{% endhighlight %}
執行結果51

以下不會執行第2個判斷句。
{% highlight java linenos %}
int age = 50;
if (age > 10 || age++ < 20) {
  System.out.println("判斷1");
}
System.out.println("age = " + age);
{% endhighlight %}
```
判斷1
age = 50
```

以下「會執行」第2個判斷句。
{% highlight java linenos %}
int age = 50;
if (age > 10 | age++ < 20) {
  System.out.println("判斷1");
}
System.out.println("age = " + age);
{% endhighlight %}
```
判斷1
age = 51
```
