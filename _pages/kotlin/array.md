---
title: array與List
date: 2025-05-15
keywords: kotlin, array, List
---
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

## arrayOf要設定值
以下程式碼編譯錯誤，因為arrayOf沒有設定值。
{% highlight kotlin linenos %}
val names = arrayOf("") // val cannot be reassigned
{% endhighlight %}

## 陣列轉List
{% highlight kotlin linenos %}
names.asList()
{% endhighlight %}

## 唯讀List
語法
```
val 變數: List<類型> = listOf(物件, 物件, 物件..)
```

只能讀取，不能修改。
{% highlight kotlin linenos %}
val names: List<String> = listOf("Tom","Jack","lucy")
for (name in names) {
    println(name)
}
{% endhighlight %}
```
Tom
Jack
lucy
```

## 可讀寫MutableList
removeAt(索引)、add(索引，值)，這二個不同Java。
{% highlight kotlin linenos %}
val mutableList = mutableListOf<String>("Alice", "Bill", "Gina")
mutableList.add("Ray")
mutableList.remove("Alice")
// 指定刪除索引位置
mutableList.removeAt(0)
// 指定插入的索引位置
mutableList.add(1,"Tina")
println(mutableList)
{% endhighlight %}
```
[Bill, Tina, Gina, Ray]
```

## 修改集合
Array
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

MutabList
{% highlight kotlin linenos %}
val mutableList = mutableListOf<String>("Alice", "Bill", "Gina")
mutableList[0] = "Gee"
println(mutableList[0])
{% endhighlight %}
```
Gee
```

## 安全取值
get(索引)可以取得元素的值，但搭配以下二種更安全。

語法
```
無此元素，就傳回預設值
names.getOrElse(索引) { "預設值"}

無此元素傳回null，使用貓王符號?:，印出預設值
names.getOrNull(索引) ?: "預設值"
```

Array
{% highlight kotlin linenos %}
 val names = arrayOf("Tom","Kevin","Lucy")
 println(names.get(0))
 println(names.getOrElse(5) { "no data"})
 println(names.getOrNull(5) ?: "no data")
{% endhighlight %}
```
Tom
no data
no data
```

List
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("======== list data ============")
println(namelist.get(0))
println(namelist.getOrElse(5) { "no data"})
println(namelist.getOrNull(5) ?: "no data")
{% endhighlight %}
```
Mary
no data
no data
```

## 索引取值
Array與List都可以用。

語法
```
${集合變數[索引]}
```

Array
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

List
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("list[0] = ${namelist[0]}")
println("list[1] = ${namelist[1]}")
println("list[2] = ${namelist[2]}")
println("list first = ${namelist.first()}")
println("list last = ${namelist.last()}")
{% endhighlight %}
```
list[0] = Mary
list[1] = Amy
list[2] = Jery
```

## first()
取出第一個元素。

Array
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("first of elements: ${names.first()}")
{% endhighlight %}
```
first of elements: Tom
```

List
{% highlight kotlin linenos %}
println("list first = ${namelist.first()}")
{% endhighlight %}
```
list first = Mary
```

## last()
取出最後一個元素。

Array
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("last of elements:${names.last()}") 
{% endhighlight %}
```
last of elements:Lucy
```

List
{% highlight kotlin linenos %}
println("list last = ${namelist.last()}")
{% endhighlight %}
```
list last = Jery
```

## 集合大小
Array
{% highlight kotlin linenos %}
val names = arrayOf("Tom","Kevin","Lucy")
println("names elements:${names.size}")
{% endhighlight %}
```
names elements:3
```

List Size
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("list size = ${namelist.size}")
{% endhighlight %}
```
list size = 3
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

{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("list contains = ${namelist.contains("Amy")}")
{% endhighlight %}
```
list contains = true
```

## 遍歷集合
for裡面的i變數，前面不會有val或var，也不會有變數類型，只要記住以下3種for即可。

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