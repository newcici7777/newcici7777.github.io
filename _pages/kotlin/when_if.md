---
title: 區間in與when、if
date: 2025-05-14
keywords: kotlin, in, when, if
---
## in
在某個範圍內。

## !in
not in，不在某個範圍內。

## 全包區間..
0 .. 6 代表包含0到包含6
{% highlight kotlin linenos %}
//in關鍵字會將變數i的值限制在指定範圍內
for (i in 0 .. 6) {
    //i在每次徝環時取得一個+1的值
    println(i)
}
{% endhighlight %}
```
0
1
2
3
4
5
6
```

{% highlight kotlin linenos %}
for (c in 'a' .. 'e') {
    //c在每次徝環時取得一個+1的值
    println(c)
}
{% endhighlight %}
```
a
b
c
d
e
```

{% highlight kotlin linenos %}
val range1 = 1 .. 10
var c = 10
println(c in range1) //true
c = 11
println(c !in range1)
{% endhighlight %}
```
true
true
```

## 半包區間until
不包含尾端的值

{% highlight kotlin linenos %}
for(i in 0 until 6) {
    println(i)
}
{% endhighlight %}
```
1
2
3
4
5
```

判斷是否有在範圍內。
{% highlight kotlin linenos %}
val range1 = 1 until 10
var c = 10
println(c in range1)
println(c !in range1)
{% endhighlight %}
```
false
true
```

## downTo
徝環順序要反過來由大到小，使用downTo
{% highlight kotlin linenos %}
for(i in 6 downTo 0) {
    println(i)
}
{% endhighlight %}
```
6
5
4
3
2
1
0
```

## step
每次跳2位數
{% highlight kotlin linenos %}
for(i in 0..6 step 2) {
    println(i)
}
{% endhighlight %}
```
0
2
4
6
```
{% highlight kotlin linenos %}
val charRange = 'a' .. 'z' //全包區間
val intRange = 1 until 10 //半包區間，不包含10
val downtoRange = 10 .. 1
val stepRange = 1 .. 10 step 2
{% endhighlight %}
