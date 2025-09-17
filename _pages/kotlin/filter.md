---
title: Filter
date: 2025-09-16
keywords: kotlin, filter
---
## filter篩選
it是list的元素，元素中有a的字母，加到新的list中，把新的list作為傳回值傳回。<br>
{% highlight kotlin linenos %}
fun main() {
    val list = listOf<String>("Mary", "Bill", "Alex", "Ben")
        .filter { it.contains("a") }
    println(list)
}
{% endhighlight %}

## filter擴展函式
filter擴展函式，參數是predicate Lambda，傳回值是List。
{% highlight kotlin linenos %}
public inline fun <T> Iterable<T>.filter(predicate: (T) -> Boolean): List<T> {
    return filterTo(ArrayList<T>(), predicate)
}
{% endhighlight %}

## filterTo擴展函式
filterTo用來遍歷所有元素，並把每個元素作為參數，傳入predicate Lambda。<br>
{% highlight kotlin linenos %}
public inline fun <T, C : MutableCollection<in T>> Iterable<T>.filterTo(destination: C, predicate: (T) -> Boolean): C {
	// 遍歷所有元素
    for (element in this) {
        // 把每個元素作為參數，傳入predicate Lambda
        if (predicate(element)) // 若為true
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

## predicate Lambda
predicate Lambda的傳回值為true或false，若為true就加到傳回值destination List中。<br>
T參數為集合中的每一個元素。<br>
{% highlight kotlin linenos %}
predicate: (T) -> Boolean
{% endhighlight %}

number為每個元素的參數，因為參數只有一個，可以省略，用it代替number。<br>
true是傳回值，若為true就加到傳回值List中。<br>
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

若為false，若為false就不加到傳回值List中。<br>
最終沒有任何元素加入List。<br>
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

## range與filter
range遇到filter會轉成list。<br>

以下程式碼是:2到7之間可以被2整除的數。<br>
{% highlight kotlin linenos %}
val range = 2 ..7
// 參數只有一個，可以省略參數，用it代替參數
val list = range.filter { it % 2 == 0 }
println(list)
{% endhighlight %}
```
[2, 4, 6]
```

以下寫法與上面相同。
{% highlight kotlin linenos %}
// range 遇到filter會產生list
val list = (2 .. 7).filter { it % 2 == 0 }
println(list)
{% endhighlight %}
```
[2, 4, 6]
```

## 傳回二維陣列中有red的元素
flatMap可以把二維陣列變成一維陣列。<br>
若元素有包含red，把它加入到陣列中。<br>
{% highlight kotlin linenos %}
fun main() {
    val list = listOf(
        listOf("red apple", "black dog", "red fish"),
        listOf("red car", "blue sky", "blue car"))
        .flatMap { it.filter { it.contains("red") } }
    println(list)
}
{% endhighlight %}
```
[red apple, red fish, red car]
```

it是每一個一維的List。<br>
{% highlight kotlin linenos %}
it.filter { it.contains("red")
{% endhighlight %}

可以看成以下這樣的程式碼。<br>
{% highlight kotlin linenos %}
listOf("red apple", "black dog", "red fish").filter {
	it.contains("red")
}
{% endhighlight %}

filter會產生新的List，元素中有red的字串，加到新的list中，把新的list作為傳回值傳回
{% highlight kotlin linenos %}
val filterList = listOf("red apple", "black dog", "red fish")
    .filter {
        it.contains("red")
    }
println(filterList)
{% endhighlight %}
```
[red apple, red fish]
```

## 質數
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

### 產生2到(number - 1)的數字
number為list中的每個元素，產生2到(number - 1)的數字，until是不包含number。<br>

因為質數只能被1與本身整除，先要判斷是不是質數。<br>

假設要判斷8是不是質數，要從2到7(不包含8)的數字中尋找可被整除的數字，若在這個範圍內找到可以整除8的「被除數」，代表8不是質數。<br>

為何範圍是2到7，因為質數只能被1與本身的數字整除，要先排除掉1與本身能整除的數，若在2到7的範圍內，可以被2 .. 7 任何一個數字整除，代表不是質數。<br>

假設number = 8
{% highlight kotlin linenos %}
(2 until number)
{% endhighlight %}

### list存放餘數
map，會傳回跟原本集合一樣大小的集合。<br>

以下這段程式碼，假設number = 8。<br>
it為2到(number - 1)的數字。<br>
{% highlight kotlin linenos %}
(2 until  number).map { number % it }
{% endhighlight %}

例:number 為 8<br>
{% highlight kotlin linenos %}
number % it
{% endhighlight %}

產生的餘數如下:<br>

8 % 2 = 0 <br>
8 % 3 = 2 <br>
8 % 4 = 0 <br>
8 % 5 = 3 <br>
8 % 6 = 2 <br>
8 % 7 = 1 <br>

map傳回新的list，把以上的餘數都放入list中。<br>
{% highlight kotlin linenos %}
    val list = (2 until 8).map { 8 % it}
    println(list)
{% endhighlight %}
```
[0, 2, 0, 3, 2, 1]
```

### none
擴展函式，this就是List，遍歷List每個元素，判斷是否符合predicate匿名函式()的條件，符合就傳回「false」。
{% highlight kotlin linenos %}
public inline fun <T> Iterable<T>.none(predicate: (T) -> Boolean): Boolean {
    if (this is Collection && isEmpty()) return true
    for (element in this) if (predicate(element)) return false
    return true
}
{% endhighlight %}

重點在以下這個程式碼，若predicate Lambda傳回值為「true」，就「返回false」。<br>
{% highlight kotlin linenos %}
if (predicate(element)) return false
{% endhighlight %}

若list中有餘數是0，代表可以被整除，Lambda傳回值為true，none()就返回false。<br>
{% highlight kotlin linenos %}
.none{ it == 0}
{% endhighlight %}

### filter過濾false的元素
predicate Lambda會傳回true或false，若為true就加到傳回值List中。<br>
若為false，就不加入傳回值List中。<br>
{% highlight kotlin linenos %}
fun main() {
    val nums = listOf(7, 5, 4, 3, 22, 11, 18)
    val prime = nums.filter { number ->
        // 質數程式碼最後傳回true 或 false，若為true就加入新list
        // 若為false就不加入新list
        false
    }
    println(prime)
}
{% endhighlight %}

## 質數2
寫一個擴展函式，判斷數字是不是質數。
{% highlight kotlin linenos %}
fun Int.isPrime(): Boolean {
    for (i in 2 until this) {
        if (this % i == 0) {
            return false
        }
    }
    return true
}
{% endhighlight %}

寫一個程式，判斷1 .. 5000之間有多少個質數，取前面10個質數出來。
{% highlight kotlin linenos %}
fun main() {
    val list = (1 .. 5000).filter { it.isPrime() }.take(10)
    println(list)
}
{% endhighlight %}
```
[1, 2, 3, 5, 7, 11, 13, 17, 19, 23]
```

### take()
集合中，取出多少個元素。

take()擴展函式(僅寫出重要部分)
{% highlight kotlin linenos %}
    // 計數器
    var count = 0
    // 新的list
    val list = ArrayList<T>(n)
    for (item in this) {
        // 加入元素
        list.add(item)
        // 若取出數量 == n，就離開迴圈。
        if (++count == n)
            break
    }
{% endhighlight %}

## Sequence序列
序列是使用時才會產生，不會先建立一個List，占住記憶體空間。<br>

產生從2開始，每個數字加\+1，產生20個，產生2、3、4 ... 19、20、21，共20個數字。<br>
generateSequence(2)，參數名為seed，也就是種子，從2開始。<br>

以下程式碼，當呼叫take(20)，才會真正產生序列的物件，建立20個元素。<br>
{% highlight kotlin linenos %}
val sequence = generateSequence(2){ value -> value + 1 }.take(20)
println(sequence.toList())
{% endhighlight %}
```
[2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21]
```

value就是從種子2，每一個step都是+1，在此步驟開始產生序列物件，每遍歷一次產生一個元素。
{% highlight kotlin linenos %}
{ value -> value + 1 }
{% endhighlight %}

以下程式碼，從2開始，產生20個質數。<br>
先判斷數字是質數，才開始產生序列元素，直到元素達到20個。<br>
{% highlight kotlin linenos %}
fun Int.isPrime(): Boolean {
    for (i in 2 until this) {
        if (this % i == 0) {
            return false
        }
    }
    return true
}

fun main() {
    val sequence = generateSequence(2){ value -> value + 1 }
        .filter { it.isPrime() }
        .take(20)
    println(sequence.toList())
}
{% endhighlight %}
```
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71]
```