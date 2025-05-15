---
title: 常用函式庫
date: 2025-05-14
keywords: kotlin, stdlib
---
## 字串
{% highlight kotlin linenos %}
fun main() {
var str = "Hello World"
println("字元個數 = ${str.count()}")
println("字串長度 = ${str.length}")
println("大寫 = ${str.uppercase()}")
println("小寫 = ${str.lowercase()}")
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