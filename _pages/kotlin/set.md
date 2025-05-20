---
title: Set
date: 2025-05-20
keywords: kotlin, Set
---
Set沒有順序，不允許重複值的集合。

## 唯讀Set
{% highlight kotlin linenos %}
val set1: Set<String> = setOf("Mary", "Mary", "Mary", "Alice")
println(set1)
{% endhighlight %}
```
[Mary, Alice]
```

## 可讀寫MutableSet
{% highlight kotlin linenos %}
val mutableSet1: MutableSet<String> = mutableSetOf("Mary", "Bill", "Jery")
{% endhighlight %}

## 取得元素
{% highlight kotlin linenos %}
println(set1.elementAt(0))
println(set1.elementAtOrElse(5) { "no data" })
println(set1.elementAtOrNull(5) ?: "no data")
{% endhighlight %}
```
Mary
no data
no data
```

## 新增刪除
Set沒有順序，因此沒辦法用索引進行修改。

新增
{% highlight kotlin linenos %}
val mutableSet1: MutableSet<String> = mutableSetOf("Mary", "Bill", "Jery")
mutableSet1.add("May")
mutableSet1 += "Gigi"
println(mutableSet1)
{% endhighlight %}

刪除
{% highlight kotlin linenos %}
mutableSet1.remove("Bill")
mutableSet1 -= "Mary"
println(mutableSet1)
{% endhighlight %}

## list轉成set去掉重覆元素
{% highlight kotlin linenos %}
val list1 = listOf("Alice", "Alice", "Alice","Mary")
    .toSet()
    .toList()
println("list1 = $list1")
{% endhighlight %}
```
list1 = [Alice, Mary]
```

distinct也有同樣效果。
{% highlight kotlin linenos %}
val list2 = listOf("Alice", "Alice", "Alice","Mary")
    .distinct()
println("list2 = $list2")
{% endhighlight %}
```
list2 = [Alice, Mary]
```