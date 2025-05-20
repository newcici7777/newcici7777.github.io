---
title: String函式
date: 2025-05-20
keywords: kotlin, String
---
## 字串
{% highlight kotlin linenos %}
fun main() {
var str = "Hello World"
println("字元個數 = ${str.count()}")
println("字串長度 = ${str.length}")
println("大寫 = ${str.uppercase()}")
println("小寫 = ${str.lowercase()}")
println("第一個字大寫 = ${str.capitalize()}")
// 注意！replace不會修改原來的值
// str.replace("o","c")
// 原來的變數去接收replace()函式的結果
str = str.replace("o", "C")
println("把o取代成C = $str")
{% endhighlight %}
```
字元個數 = 11
字串長度 = 11
大寫 = HELLO WORLD
小寫 = hello world
把o取代成C = Hello World
```

判斷字串是否為空
{% highlight kotlin linenos %}
var s1 = "871"
println(s1.isNotBlank())
{% endhighlight %}

{% highlight kotlin linenos %}
var s1 = "871"
println(s1.toSortedSet())//對字串排序
//[1, 7, 8]

var s2 = "871aka"
println(s2.toSortedSet())
//[1, 7, 8, a, k]
//排序後剩下一個a，因為是set
{% endhighlight %}

可以透過get與\[\]拿字串中的字母
{% highlight kotlin linenos %}
var s = "asd"
println(s.get(2))
//d

println(s[2])
//d
{% endhighlight %}

字串與數字連接
{% highlight kotlin linenos %}
val b = 1
//可直接連接數字，變成字串
val s3 = "876"+ 1 +"aka"
println(s3) //8761aka
//但改用$字號連接字串
val s4 = "876${b}aka"//要相連一起要使用花括號才不會把aka也視為變數的其中之一
//預設會把$符號後面字母設為一個變數
{% endhighlight %}