---
title: 算術運算子與轉型
date: 2025-06-18
keywords: Java, operator
---
## int與double轉型
## 範例1
{% highlight java linenos %}
  double num = 10 / 4;
  System.out.println(num);    
{% endhighlight %}

以上程式碼要分二部分，首先是int整數相除與無條件捨去小數點。
{% highlight java linenos %}
System.out.println(10 / 4);
{% endhighlight %}
執行結果為2

第二部分是把整數2強制轉型成double。
{% highlight java linenos %}
double num = 2;
{% endhighlight %}
執行結果為2.0

## 範例2
{% highlight java linenos %}
System.out.println(10.0 / 4);
{% endhighlight %}
算式中有double(10.0)，就會把int(4)轉成double，變成double的算式。

執行結果為2.5

## 範例3
{% highlight java linenos %}
System.out.println(10 / 4);
{% endhighlight %}
二個都為整數，無條件捨去小數點。

執行結果為2。