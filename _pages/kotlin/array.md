---
title: array與List
date: 2025-05-15
keywords: kotlin, array, List
---
## 陣列宣告
{% highlight kotlin linenos %}
val arr = arrayOf(1, 2)
{% endhighlight %}

## 印出值
語法
```
${陣列變數[索引]}
```
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("First elements:${names[0]}")  // First elements:Tom
println("First elements:${names[1]}")  // First elements:Kevin
println("First elements:${names[2]}")  // First elements:Lucy
//println("First elements:${names[5]}")  // Index 5 out of bounds for length 3
{% endhighlight %}

## last
使用last表示最後一個元素位置
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("last of elements:${names.last()}")  // last of elements:Lucy
println("names elements:${names.size}")  // names elements:3
{% endhighlight %}

## arrayOf要設定值
以下程式碼編譯錯誤，因為arrayOf沒有設定值。
{% highlight kotlin linenos %}
val names = arrayOf("") // val cannot be reassigned
{% endhighlight %}

## 修改值
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
names[0] = "Julien"
// 原來的tom改成julien
println("First elements:${names[0]}")  // First elements:Julien
// 印出Julien第二個字母u
println("First elements:${names[0][1]}")  // First elements:u
// 印出長度
println("Length of Julien:${names[0].length}")  // Length of Julien:6
{% endhighlight %}

## 陣列可以有各種類型
{% highlight kotlin linenos %}
var number = arrayOf(1, 2, 3, 'Y')
number[0] = 'A' // char 類型
println("First elements:${number[0]}")  // First elements:A
number[0] = "TEST" // String 類型
println("First elements:${number[0]}")  // First elements:TEST
{% endhighlight %}

## 二維陣列
{% highlight kotlin linenos %}
var number2 = arrayOf(arrayOf(1, 2, 3), arrayOf(4, 5, 6), arrayOf(7, 8, 9))
println("Number:${number2[2][1]}")  //Number:8
{% endhighlight %}

## iterator
- [iterator][1]

通過iterator拿到陣列中每一項。

透過Array中的iterator()拿到 Iterator<T>的介面。
- next()取得陣列中的值。
- hasNext()是否還有下一個元素讓你拿。

{% highlight kotlin linenos %}
val arr = arrayOf(1, 2)
println(arr.iterator().next()) 
println(arr.iterator().hasNext()) 
{% endhighlight %}

## for
{% highlight kotlin linenos %}
val arr = arrayOf(1, 2)
for (i in arr) {
    println(i)
}
{% endhighlight %}
```
1
2
```

## indices
indices為index的複數，傳回array的索引值。
{% highlight kotlin linenos %}
val arr = arrayOf(1, 2)
for (i in arr.indices) {
    println(i)
}
{% endhighlight %}
```
0
1
```

## 想拿到index跟valaue
同時想拿到index跟valaue，可以使用withIndex.返回一個對象有index跟value的屬性。
{% highlight kotlin linenos %}
val arr = arrayOf(1, 2)
for(i in arr.withIndex()) {
    println("index:${i.index}, value:${i.value}")
}
{% endhighlight %}
```
index:0, value:1
index:1, value:2
```

## foreach
{% highlight kotlin linenos %}
val arr = arrayOf(1, 2)
arr.forEach { it ->
    println(it)
}
{% endhighlight %}
```
1
2
```

## forEachIndexed
{% highlight kotlin linenos %}
val arr = arrayOf(1, 2)
arr.forEachIndexed {
    index, i ->
    println("$index $i")
}
{% endhighlight %}
```
0 1
1 2
```

## List宣告
{% highlight kotlin linenos %}
var names: List<String> = listOf("Tom","Jack","lucy")
for (name in names) {
    println(name)
}
{% endhighlight %}
```
Tom
Jack
lucy
```

要索引值使用withIndex()方法
{% highlight kotlin linenos %}
var names2: List<String> = listOf("Tom", "Jack", "lucy")
for((index,name) in names.withIndex()) {
    println("$index:$name")
}
{% endhighlight %}
```
0:Tom
1:Jack
2:lucy
```

indices會傳回陣列或集合的範圍
{% highlight kotlin linenos %}
var names: List<String> = listOf("Tom","Jack","lucy")
for(index in names.indices) {
    println("$index: ${names[index]}")
}
{% endhighlight %}
```
0: Tom
1: Jack
2: lucy
```

[1]: {% link _pages/design_pattern/iterator.md %}