---
title: Iterable<T>相關函式
date: 2025-09-16
keywords: kotlin, Functional Programming, map, flatmap
---
## Functional Programming
原本的集合不會動到，複製一份給其它函式去執行，傳回新的結果。

## Iterable<T>.map()
擴展函式程式碼如下:
{% highlight kotlin linenos %}
public inline fun <T, R> Iterable<T>.map(transform: (T) -> R): List<R> {
    return mapTo(ArrayList<R>(collectionSizeOrDefault(10)), transform)
}
{% endhighlight %}

### 集合元素字串長度
Iterable<T>.map()集合擴展函式map，會傳回跟原本集合一樣大小的集合。<br>
以下程式碼會傳回list元素的長度，傳回新的list，不會修改到原本的list。<br>
it代表每個集合元素。<br>
{% highlight kotlin linenos %}
fun main() {
    val list = listOf<String>("Mary", "Bill", "Alex", "Ben")
    val result = list.map { it.length }
    println(result)
}
{% endhighlight %}
```
[4, 4, 4, 3]
```

### 加上前綴字串
以下程式碼為每個元素加上前綴字串，不會修改原本list，會複製一份新的list，修改複本傳回。<br>
{% highlight kotlin linenos %}
fun main() {
    val list = listOf<String>("Mary", "Bill", "Alex", "Ben")
        .map { "Little " + it }
        .map { "Happy " + it }
    println(list)
}
{% endhighlight %}
```
[Happy Little Mary, Happy Little Bill, Happy Little Alex, Happy Little Ben]
```