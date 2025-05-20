---
title: Array
date: 2025-05-15
keywords: kotlin, arrayof, intArrayOf
---
陣列無法新增值，因為陣列是一個固定大小的陣列，可以修改。

## 建立陣列

|陣列類型 |建立陣列語法|
|:------ |:-------|
|IntArray    |intArrayOf    |
|DoubleArray |doubleArrayOf | 
|LongArray   |longArrayOf   |  
|ShortArray   |shortArrayOf  |   
|ByteArray   |byteArrayOf   |  
|FloatArray  |floatArrayOf  |   
|BooleanArray|booleanArrayOf|    
|Array\<物件\> |arrayOf       |  

建立int陣列
{% highlight kotlin linenos %}
val intarr: IntArray = intArrayOf(10, 20, 30)
// 可省略類型，會自動推導。
val intarr = intArrayOf(10, 20, 30)
{% endhighlight %}

建立double陣列
{% highlight kotlin linenos %}
var d1: DoubleArray = doubleArrayOf(1.0, 2.0, 3.0)
{% endhighlight %}

## 指定大小，但值全為0
指定大小，但值為0的 int array，注意！array的內容不是5。

是用大寫開頭的IntArray()建立陣列。
{% highlight kotlin linenos %}
val a3: IntArray = IntArray(5)
for(i in a3){
    println(i)
}
{% endhighlight %}
```
0
0
0
0
0
```

## arrayOf
除了用intArrayOf()，也可使用arrayOf()建立Int陣列。

arrayOf 通過\<T\>泛型，來決定陣列類型。

其中的\<Int\>不用寫，透過1,2,3的值來自動推導類型
{% highlight kotlin linenos %}
var a: Array<Int> = arrayOf<Int>(1,2,3) 
{% endhighlight %}	

改成以下的方式建立Int陣列
{% highlight kotlin linenos %}
var a: Array<Int> = arrayOf(1,2,3) 
{% endhighlight %}	

變數的類型可以再省略，透過1,2,3的值來自動推導類型
{% highlight kotlin linenos %}
var a = arrayOf(1,2,3) 
{% endhighlight %}

建立String物件陣列
{% highlight kotlin linenos %}
val arr: Array<String> = arrayOf("Tom","Kevin","Lucy")
// 可省略類型，會自動推導。
val arr = arrayOf("Tom","Kevin","Lucy")
{% endhighlight %}

## 二維陣列
{% highlight kotlin linenos %}
var number2 = arrayOf(arrayOf(1, 2, 3), arrayOf(4, 5, 6), arrayOf(7, 8, 9))
println("Number:${number2[2][1]}")
{% endhighlight %}
```
Number:8
```

## 陣列可以有各種類型
{% highlight kotlin linenos %}
var number = arrayOf(1, 2, 3, 'Y')
number[0] = 'A' // char 類型
println("First elements:${number[0]}")
number[0] = "TEST" // String 類型
println("First elements:${number[0]}") 
{% endhighlight %}
```
First elements:A
First elements:TEST
```

## 建立空的array
以下程式碼編譯錯誤，因為arrayOf沒有設定類型。
{% highlight kotlin linenos %}
val names = arrayOf("") // val cannot be reassigned
{% endhighlight %}

{% highlight kotlin linenos %}
val a2 = arrayOf<Int>()
{% endhighlight %}

想不出為什麼可以建立空的array，因為陣列是一個固定大小的陣列，建立空的陣列也沒辦法新增。

## 陣列轉List
{% highlight kotlin linenos %}
names.asList()
{% endhighlight %}

## 安全取值
get(索引)可以取得元素的值，但搭配以下二種更安全。

語法
```
無此元素，就傳回預設值
集合變數.getOrElse(索引) { "預設值"}

無此元素傳回null，使用貓王運算子?:，印出預設值
集合變數.getOrNull(索引) ?: "預設值"
```

{% highlight kotlin linenos %}
 val names = arrayOf("Tom","Kevin","Lucy")
 println(names.get(0))
 println(names.getOrElse(5) { "no data" })
 println(names.getOrNull(5) ?: "no data")
{% endhighlight %}
```
Tom
no data
no data
```

## 索引取值
語法
```
${變數[索引]}
```

{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("${names[0]}")  
println("${names[1]}")  
println("${names[2]}") 
// 超出範圍 
//println("${names[5]}")  
// Index 5 out of bounds for length 3
{% endhighlight %}
```
Tom
Kevin
Lucy
```

## 修改
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
names[0] = "Julien"
// 原來的tom改成julien
println("[0]:${names[0]}")  
// 印出Julien第二個字母u
println("[0][1]:${names[0][1]}")  
// 印出長度
println("Length of Julien:${names[0].length}")
{% endhighlight %}
```
[0]:Julien
[0][1]:u
Length of Julien:6
```

## first()
取出第一個元素。

{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("first of elements: ${names.first()}")
{% endhighlight %}
```
first of elements: Tom
```
## last()
取出最後一個元素。

{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("last of elements:${names.last()}") 
{% endhighlight %}
```
last of elements:Lucy
```

## size
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("names elements:${names.size}")
{% endhighlight %}
```
names elements:3
```

## indexOf
搜尋在第幾個索引，找不到傳回-1
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("search index = ${namelist.indexOf("Amy")}")
{% endhighlight %}
```
search index = 1
```

## contains
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("array contains = ${names.contains("Tom")}")
{% endhighlight %}
```
array contains = true
```

## in 
與contains功能一樣，都有包含的意思。
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("Tom" in names)
{% endhighlight %}
```
true
```

## 遍歷集合
for裡面的i變數，前面不會有val或var，也不會有變數類型，只要記住以下3種for即可。

IntelliJ快速鍵: 集合變數.for

### for
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

### foreach
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

### forEachIndexed
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

## 其它遍歷方式
只要記得上面3種for，下面若仍有腦容量再記住吧。
### iterator
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

### indices
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

### 想拿到index跟valaue
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