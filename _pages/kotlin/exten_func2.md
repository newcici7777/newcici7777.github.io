---
title: Iterable<T>相關函式
date: 2025-09-16
keywords: kotlin, Functional Programming, map, flatmap
---
## Functional Programming
原本的集合不會動到，複製一份給其它函式去執行，傳回新的結果。

## Iterable<T>.擴展函式()
Iterable是集合，List、Set都是Iterable的子類別，就可以用Iterable<T>.擴展函式()。<br>

以下建立取出第1個元素的泛型擴展函式，並讓list與set使用。<br>
{% highlight kotlin linenos %}
fun <T> Iterable<T>.getFirst(): T {
    return this.first()
}

fun main() {
    val list = listOf<String>("Mary", "Alex", "Bill")
    val set1: Set<String> = setOf("Alice", "Mary", "Mary", "Alice")
    println(list.getFirst())
    println(set1.getFirst())
}
{% endhighlight %}
```
Mary
Alice
```

## Iterable是一個Interface，子類別list、set、map會實作iterator()，可以使用Range與for()迴圈，遍歷每個元素。
{% highlight kotlin linenos %}
public interface Iterable<out T> {
    // Returns an iterator over the elements of this object.
    public operator fun iterator(): Iterator<T>
}
{% endhighlight %}

## IntRange
IntRage繼承IntProgression，IntProgression實作Iterator，實作hasNext()、nextInt()函式，遍歷每個元素。
{% highlight kotlin linenos %}
class IntRange(start: Int, end: Int) : IntProgression(start, end, 1) {
    override fun iterator(): IntIterator {
        return object : IntIterator() {
            private var current = first
            override fun hasNext(): Boolean = current <= last
            override fun nextInt(): Int = current++
        }
    }
}
{% endhighlight %}

{% highlight kotlin linenos %}
for (i in 2 .. 7) {
    print("$i ")     // 輸出: 2 3 4 5 6 7
}
{% endhighlight %}

## Iterable<T>.map()擴展函式
Iterable<T>.map()擴展函式，會傳回跟原本集合「一樣大小」的List。<br>
擴展函式程式碼如下:<br>
{% highlight kotlin linenos %}
public inline fun <T, R> Iterable<T>.map(transform: (T) -> R): List<R> {
    return mapTo(ArrayList<R>(collectionSizeOrDefault(10)), transform)
}
{% endhighlight %}

### 集合元素字串長度
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
以下程式碼為每個元素加上前綴字串，不會修改原本list，會複製一份新的lis，修改複本傳回。<br>
傳回的list的大小跟原本的list的大小一樣。<br>
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

## flatMap
flatMap主要是把二維List變成一維List。<br>
以下是二維List。<br>
{% highlight kotlin linenos %}
val list2d = listOf(listOf(1,2,3), listOf(4,5,6))
{% endhighlight %}

flatMap擴展函式，傳入一個空的ArrayList到flatMapTo()函式。
{% highlight kotlin linenos %}
public inline fun <T, R> Iterable<T>.flatMap(transform: (T) -> Iterable<R>): List<R> {
    return flatMapTo(ArrayList<R>(), transform)
}
{% endhighlight %}

實際真正做事的擴展函式是以下這個。<br>
this是二維List，二維List每個元素都是一維陣列，使用for遍歷二維List，element就是一維陣列。
{% highlight kotlin linenos %}
public inline fun <T, R, C : MutableCollection<in R>> Iterable<T>.flatMapTo(destination: C, transform: (T) -> Iterable<R>): C {
    for (element in this) {
        val list = transform(element)
        destination.addAll(list)
    }
    return destination
}
{% endhighlight %}

二維List
{% highlight kotlin linenos %}
val list2d = listOf(listOf(1,2,3), listOf(4,5,6))
{% endhighlight %}

第1次循環的element是
{% highlight kotlin linenos %}
listOf(1,2,3)
{% endhighlight %}

第2次循環的element是
{% highlight kotlin linenos %}
listOf(4,5,6)
{% endhighlight %}

把每一個元素(一維List)，加入到新的ArrayList中。
{% highlight kotlin linenos %}
destination.addAll(list)
{% endhighlight %}

### transform Lambda
每個一維List，傳入transform Lambda。<br>
{% highlight kotlin linenos %}
val list = transform(element)
{% endhighlight %}

transform類型: (T) -> Iterable<R>，傳入參數為一維List，傳回值是Iterable介面，只要繼承Iterable的子類都可以作為傳回值。<br>

以下Lambda{}，參數是一維的list，傳回值也是一維的list。<br>
list也是Iterable的子類。<br>
{% highlight kotlin linenos %}
val list2d = listOf(listOf(1,2,3), listOf(4,5,6))
val list = list2d.flatMap { innerList -> innerList }
{% endhighlight %}

因為參數只有一個，可以用it來取代，it就是一維的list，傳回一維的list。<br>
{% highlight kotlin linenos %}
val list2d = listOf(listOf(1,2,3), listOf(4,5,6))
val list = list2d.flatMap { it }
{% endhighlight %}

完整程式碼:把二維List變成一維List
{% highlight kotlin linenos %}
fun main() {
    val list = listOf(
        listOf(1, 2, 3),
        listOf(4, 5, 6)).flatMap { it }
    println(list)
}
{% endhighlight %}
```
[1, 2, 3, 4, 5, 6]
```

