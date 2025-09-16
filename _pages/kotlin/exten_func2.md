---
title: Iterable<T>相關函式
date: 2025-09-16
keywords: kotlin, Functional Programming, map, flatmap
---
## Functional Programming
原本的集合不會動到，複製一份給其它函式去執行，傳回新的結果。

## Iterable<T>.map()擴展函式
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
flat是平的意思。<br>
以下是二維陣列，把二維陣列變成一維陣列，使用flatMap。<br>
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

## filter篩選
it是list的元素，元素中有a的字母，加到新的list中，把新的list作為傳回值傳回。<br>
{% highlight kotlin linenos %}
fun main() {
    val list = listOf<String>("Mary", "Bill", "Alex", "Ben")
        .filter { it.contains("a") }
    println(list)
}
{% endhighlight %}

### filter擴展函式
filter擴展函式，參數是predicate Lambda，傳回值是List。
{% highlight kotlin linenos %}
public inline fun <T> Iterable<T>.filter(predicate: (T) -> Boolean): List<T> {
    return filterTo(ArrayList<T>(), predicate)
}
{% endhighlight %}

### filterTo擴展函式
{% highlight kotlin linenos %}
public inline fun <T, C : MutableCollection<in T>> Iterable<T>.filterTo(destination: C, predicate: (T) -> Boolean): C {
	// 遍歷所有元素
    for (element in this) {
        // 若為true
        if (predicate(element)) 
            // element 加入 destination List
            destination.add(element)
    }
    return destination
}
{% endhighlight %}

重要的是下面這段程式
{% highlight kotlin linenos %}
// 遍歷所有元素
for (element in this) {
    // 若為true
    if (predicate(element)) 
        // element 加入 destination List
        destination.add(element)
}
{% endhighlight %}

### predicate Lambda
predicate Lambda會傳回true或false，若為true就加到傳回值List中。<br>
{% highlight kotlin linenos %}
predicate: (T) -> Boolean
{% endhighlight %}

number為每個元素的參數，因為參數只有一個，可以省略，用it代替number。<br>
{% highlight kotlin linenos %}
val nums = listOf(7, 5, 4, 3, 22, 11, 18)
    .filter { number ->
        true
    }
println(nums)
{% endhighlight %}
```
[7, 5, 4, 3, 22, 11, 18]
```

若為false，就沒有任何元素加入List。<br>
{% highlight kotlin linenos %}
val nums = listOf(7, 5, 4, 3, 22, 11, 18)
    .filter { number ->
        false
    }
println(nums)
{% endhighlight %}
```
[]
```

### 傳回二維陣列中有red的元素
以下是二維陣列，傳回一維陣列，若元素有包含red，把它加入到陣列中。<br>
{% highlight kotlin linenos %}
fun main() {
    val list = listOf(
        listOf("red apple", "black dog", "red fish"),
        listOf("yellow car", "blue sky", "blue car"))
        .flatMap { it.filter { it.contains("red") } }
    println(list)
}
{% endhighlight %}
```
[red apple, red fish]
```

### 質數
質數只能被1與自己整除。<br>
{% highlight kotlin linenos %}
fun main() {
    val nums = listOf(7, 5, 4, 3, 22, 11, 18)
    val prime = nums.filter { number ->
        (2 until  number).map { number % it }
            .none{ it == 0}
    }
    println(prime)
}
{% endhighlight %}
```
[7, 5, 3, 11]
```

#### 產生2到(number - 1)的數字
number為list中的每個元素，產生2到(number - 1)的數字，until是不包含number。<br>
因為質數只能被1與本身整除，所以要從2到(本身 - 1)的數字中尋找可被整除的數字。<br>

{% highlight kotlin linenos %}
(2 until number)
{% endhighlight %}

#### list存放餘數
map，會傳回跟原本集合一樣大小的集合。<br>
it為2到(number - 1)的數字，number是nums list的元素，別搞錯。<br>
例:number 為 7<br>
7 % 2 = 1 <br>
7 % 3 = 1 <br>
7 % 4 = 3 <br>
7 % 5 = 2 <br>
7 % 6 = 1 <br>
map傳回新的list，把以上的餘數都放入list中。<br>
{% highlight kotlin linenos %}
(2 until number).map { number % it}
{% endhighlight %}

#### none
擴展函式
{% highlight kotlin linenos %}
public inline fun <T> Iterable<T>.none(predicate: (T) -> Boolean): Boolean {
    if (this is Collection && isEmpty()) return true
    for (element in this) if (predicate(element)) return false
    return true
}
{% endhighlight %}

重點在以下這個程式碼，若predicate Lambda傳回值為true，就返回false。<br>
{% highlight kotlin linenos %}
if (predicate(element)) return false
{% endhighlight %}

若list中有餘數是0，代表可以被整除，Lambda傳回值為true，none()就返回false。<br>
{% highlight kotlin linenos %}
.none{ it == 0}
{% endhighlight %}

#### filter過濾false的元素
predicate Lambda會傳回true或false，若為true就加到傳回值List中。<br>
若為false，就不加入傳回值List中。<br>