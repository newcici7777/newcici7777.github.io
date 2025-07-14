---
title: Map,MutableMap
date: 2025-05-20
keywords: kotlin, Map, MutableMap
---
## to函式
to區分key與value。

以下是to函式原始碼，傳回值是一個Pair的類別。
{% highlight java linenos %}
public infix fun <A, B> A.to(that: B): Pair<A, B> = Pair(this, that)
{% endhighlight %}

也就是說，以下是在呼叫to的函式，傳回Pair，Pair再放入Map中，Map裡存放的都是Pair。
{% highlight kotlin linenos %}
"Alice" to 18
{% endhighlight %}

## Pair
可透過first與second取值
{% highlight kotlin linenos %}
val pair = Pair<String, Int>("Alex", 12)
println("first = ${pair.first} second = ${pair.second}")
{% endhighlight %}
```
first = Alex second = 12
```

## 唯讀Map
以下key是姓名與value是年齡。
{% highlight kotlin linenos %}
val map1 = mapOf(
    "Alice" to 18,
    "Alex" to 20,
    "Momo" to 5,
    "Yoyo" to 8
)
{% endhighlight %}

使用Pair，以下等同上面。
{% highlight kotlin linenos %}
val map3 = mapOf(
    Pair("Alice", 18),
    Pair("Alex", 20),
    Pair("Momo", 5),
    Pair("Yoyo", 8)
)
{% endhighlight %}

## 可讀寫MutableMap
{% highlight kotlin linenos %}
val mutableMap = mutableMapOf(
    "Alice" to 18,
    "Alex" to 20,
    "Momo" to 5,
    "Yoyo" to 8
)
{% endhighlight %}

## 讀取
- \[key\] 沒有鍵\/值，系統就會傳回null。
- get(key) 沒有鍵\/值，系統就會傳回null。
- getOrDefault("Momo", 0) 沒有鍵\/值就傳回預設值。
- getOrElse("Yoyo") \{0\} 沒有鍵\/值就傳回預設值。

{% highlight kotlin linenos %}
val mutableMap = mutableMapOf(
    "Alice" to 18,
    "Alex" to 20,
)
println("Alice age = ${mutableMap["Alice"]}")
println("Alex age = ${mutableMap.get("Alex")}")
println("Momo age = ${mutableMap.getOrDefault("Momo", 0)}")
println("Yoyo age = ${mutableMap.getOrElse("Yoyo") {0}}")
{% endhighlight %}
```
Alice age = 18
Alex age = 20
Momo age = 0
Yoyo age = 0
```

## forEach
{% highlight kotlin linenos %}
val mutableMap = mutableMapOf(
    "Alice" to 18,
    "Alex" to 20,
    "Momo" to 5,
    "Yoyo" to 8
)
mutableMap.forEach{
    println("key = ${it.key} , value = ${it.value}")
}    
{% endhighlight %}
```
key = Alice , value = 18
key = Alex , value = 20
key = Momo , value = 5
key = Yoyo , value = 8
```

{% highlight kotlin linenos %}
mutableMap.forEach { key, value ->
    println("key = $key , value = $value")
}
{% endhighlight %}

## 新增
{% highlight kotlin linenos %}
val mutableMap = mutableMapOf(
    "Alice" to 18,
    "Alex" to 20,
    "Momo" to 5,
    "Yoyo" to 8
)
mutableMap += "Bob" to 10
mutableMap.put("Ray", 25)
println(mutableMap)
{% endhighlight %}
```
{Alice=18, Alex=20, Momo=5, Yoyo=8, Bob=10, Ray=25}
```

沒有這個key，才新增，執行結果發現Momo仍是5歲，不是12歲。
{% highlight kotlin linenos %}
mutableMap.getOrPut("Elsa") { 12 }
mutableMap.getOrPut("Momo") { 12 }
{% endhighlight %}
```
{Alice=18, Alex=20, Momo=5, Yoyo=8, Bob=10, Ray=25, Elsa=12}
```

## 修改
{% highlight kotlin linenos %}
mutableMap[key] = value
mutableMap["Momo"] = 10
{% endhighlight %}

## 刪除
{% highlight kotlin linenos %}
mutableMap.remove(key)
mutableMap.remove("Momo")
{% endhighlight %}
