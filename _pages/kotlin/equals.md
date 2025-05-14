---
title: 記憶體位址是否相同
date: 2025-05-14
keywords: kotlin, equals, compare
---
## ==
二個等於是比較「值」。

## 非空類型===
如果是基本資料型別String, Int, Double, Float, Long, Short, Char, Byte, Boolean

比較的仍是「值」。

## 可空類型===
「可空類型」，比較的是「記憶體位址」。

要繼續讀下面的內容，一定要先了解共享模式，Integer與String pool。
- [共享模式][1]

### Integer
值為127，記憶體位址都是相等。
{% highlight java linenos %}
println("=========非空類型127=============")
val x = 127
val y = 127
println("位址相等 = ${x === y}")
println("值相等 = ${x == y}")
println("=========可空類型127=============")
val x1:Int? = 127
val y1:Int? = 127
println("位址相等 = ${x1 === y1}")
println("值相等 = ${x1 == y1}")
println("=========可空與非空類型127=============")
println("位址相等 = ${x1 === x}")
println("值相等 = ${x1 == x}")
{% endhighlight %}
```
=========非空類型127=============
位址相等 = true
值相等 = true
=========可空類型127=============
位址相等 = true
值相等 = true
=========可空與非空類型127=============
位址相等 = true
值相等 = true
```

超出128，以下記憶體位址只有非空類型是相等，可空類型都不是相等。
{% highlight java linenos %}
println("=========非空類型128=============")
val x = 128
val y = 128
println("位址相等 = ${x === y}")
println("值相等 = ${x == y}")
println("=========可空類型128=============")
val x1:Int? = 128
val y1:Int? = 128
println("位址相等 = ${x1 === y1}")
println("值相等 = ${x1 == y1}")
println("=========可空與非空類型128=============")
println("位址相等 = ${x1 === x}")
println("值相等 = ${x1 == x}")
{% endhighlight %}
```
=========非空類型128=============
位址相等 = true
值相等 = true
=========可空類型128=============
位址相等 = false
值相等 = true
=========可空與非空類型128=============
位址相等 = false
值相等 = true
```

### String
以下程式碼，不管是非空類型與可空類型，都是指向同一個記憶體位址，指向同一個String Pool中的常數。
{% highlight java linenos %}
val str1 = "Jack"
val str2 = "Jack"
println("位址相等 = ${str1 === str2}")
println("值相等 = ${str1 == str2}")
val s1:String? = "Jack"
val s2:String? = "Jack"
println("位址相等 = ${s1 === s2}")
println("值相等 = ${s1 == s2}")
{% endhighlight %}
```
位址相等 = true
值相等 = true
位址相等 = true
值相等 = true
```


[1]: {% link _pages/design_pattern/flyweight.md %}
