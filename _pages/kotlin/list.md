---
title: List與MutableList
date: 2025-05-20
keywords: kotlin, List, MutableList
---
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

## 建立可讀寫MutableList
建立MutableList
{% highlight kotlin linenos %}
val mutableList = mutableListOf<String>("Alice", "Bill", "Gina")
{% endhighlight %}

建立空的MutableList
{% highlight kotlin linenos %}
val mutableList:MutableList<Int> = mutableListOf()
{% endhighlight %}

## MutableList新增刪除修改
新增
{% highlight kotlin linenos %}
mutableList.add("Ray")
// 指定插入的索引位置
mutableList.add(1,"Tina")
{% endhighlight %}

刪除
{% highlight kotlin linenos %}
mutableList.remove("Alice")
// 指定刪除索引位置
mutableList.removeAt(0)
{% endhighlight %}

刪除前判斷有存在才刪。
{% highlight kotlin linenos %}
val mutableList = mutableListOf<String>("Alice", "Bill", "Gina")
mutableList.removeIf{it.contains("Alice") }
println(mutableList)
{% endhighlight %}
```
[Bill, Gina]
```

清空
{% highlight kotlin linenos %}
mutableList.clear()
println(mutableList)
{% endhighlight %}
```
[]
```

## 安全取值
get(索引)可以取得元素的值，但搭配以下二種更安全。

語法
```
無此元素，就傳回預設值
集合變數.getOrElse(索引) { "預設值"}

無此元素傳回null，使用貓王運算子?:，印出預設值
集合變數s.getOrNull(索引) ?: "預設值"
```

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
語法
```
${變數[索引]}
```
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

## 修改集合
{% highlight kotlin linenos %}
val mutableList = mutableListOf<String>("Alice", "Bill", "Gina")
mutableList[0] = "Gee"
println(mutableList[0])
{% endhighlight %}
```
Gee
```

## mutator
能用在MutableList，Array無法使用。

語法
```
+= 新增元素
-= 刪除元素
```

新增
{% highlight kotlin linenos %}
val mutableList = mutableListOf<String>("Alice", "Bill", "Gina")
mutableList += "Momo"
mutableList += "Yoyo"
println(mutableList)
{% endhighlight %}
```
[Alice, Bill, Gina, Momo, Yoyo]
```

刪除
{% highlight kotlin linenos %}
val mutableList = mutableListOf<String>("Alice", "Bill", "Gina")
mutableList += "Momo"
mutableList += "Yoyo"
println(mutableList)
mutableList -= "Momo"
mutableList -= "Yoyo"
println(mutableList)
{% endhighlight %}
```
[Alice, Bill, Gina]
```

## List與MutableList互轉
List轉MutableList
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
namelist.toMutableList()
{% endhighlight %}

MutableList轉List
{% highlight kotlin linenos %}
mutableList.toList()
{% endhighlight %}

## first()
取出第一個元素。
{% highlight kotlin linenos %}
println("list first = ${namelist.first()}")
{% endhighlight %}
```
list first = Mary
```

## last()
取出最後一個元素。
{% highlight kotlin linenos %}
println("list last = ${namelist.last()}")
{% endhighlight %}
```
list last = Jery
```

## size
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("list size = ${namelist.size}")
{% endhighlight %}
```
list size = 3
```

## contains
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("list contains = ${namelist.contains("Amy")}")
{% endhighlight %}
```
list contains = true
```

## in 
與contains功能一樣，都有包含的意思。
{% highlight kotlin linenos %}
val namelist = listOf<String>("Mary", "Amy", "Jery")
println("Mary" in namelist)
{% endhighlight %}
```
true
```

## empty
{% highlight kotlin linenos %}
val mutableList:MutableList<Int> = mutableListOf()
if (mutableList.isEmpty())
    println("empty")
if (mutableList.isNotEmpty())
    println("not empty")
{% endhighlight %}
```
empty
```

## distinct()去掉重覆
{% highlight kotlin linenos %}
val mutableList1 = mutableListOf<String>("Alice", "Bill", "Gina", "Alice")
println(mutableList1)
val distinctList  = mutableList1.distinct()
println(distinctList)
{% endhighlight %}
```
[Alice, Bill, Gina, Alice]
[Alice, Bill, Gina]
```

## 遍歷集合
for裡面的i變數，前面不會有val或var，也不會有變數類型。

內容請見陣列中的遍歷集合。