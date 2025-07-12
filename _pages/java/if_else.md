---
title: if elseif else
date: 2025-07-12
keywords: Java, if else elseif
---
## 語法
```
if (true) {
  執行程式碼
} else if (true) {
  執行程式碼
} else {
  以上都不是才執行	
}
```
if()與else if()，只能接受boolean為「true」，才執行。

## boolean
以下程式碼印出結果？
{% highlight java linenos %}
boolean flag = true;
if (flag = false) {
  System.out.println("flow1");
} else if (flag) {
  System.out.println("flow2");
} else if (!flag){
  System.out.println("flow3");
} else {
  System.out.println("flow4");
}
{% endhighlight %}
```
flow3
```

可以透過以下方式，在判斷條件的位置，指派值給變數。<br>
此時flag變成false，if(false)，就不進入以下的條件判斷。<br>
{% highlight java linenos %}
if (flag = false)
{% endhighlight %}

!變相反，原本flag是false，!相反後變成true，進入這個if(true)條件判斷。
{% highlight java linenos %}
if (!flag)
{% endhighlight %}

只要有true，執行完`if(true) { 程式碼; }`就離開條件判斷句。<br>
![img]({{site.imgurl}}/java/if_elseif.png) <br>

以下程式碼執行結果如何？
{% highlight java linenos %}
boolean flag = true;
if (flag == false) {
  System.out.println("flow1");
} else if (flag) {
  System.out.println("flow2");
} else if (!flag){
  System.out.println("flow3");
} else {
  System.out.println("flow4");
}
{% endhighlight %}
```
flow2
```

此時flag仍然是true，會執行以下if(true)條件判斷句。
{% highlight java linenos %}
if (flag)
{% endhighlight %}

## range
- [開區間 閉區間 半開區間][1]

使用開區間、閉區間的方式來判斷以下的區間。
{% highlight java linenos %}
int score = 80;
if (score >= 90 && score <= 100) {
  // [90, 100]  90 <= score <= 100
  System.out.println("優");
} else if (score >= 80 && score < 90) {
  // [80, 90) 80 <= score < 90
  System.out.println("甲");
} else if (score >= 70 && score < 80){
  // [70, 80) 70 <= score < 80
  System.out.println("乙");
} else {
  // [0, 70) 0 <= score < 70
  System.out.println("丙");
}
{% endhighlight %}
```
甲
```

## 判斷閏年
不知道為什麼很愛考這題，實在非常非常討厭這題，每次都忘記條件，所以在這邊特別記下來。

符合以下2個條件就是閏年。<br>
1. 年份能被4整除，但不能被100整除。
2. 能被400整除。

{% highlight java linenos %}
int year = 2025;
if ((year % 4 == 0 && year % 100 != 0)
    || year % 400 == 0) {
  System.out.println("閏年");
} else {
  System.out.println("不是閏年");
}
{% endhighlight %}

[1]: {% link _pages/math/range.md %}


