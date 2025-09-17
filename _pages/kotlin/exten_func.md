---
title: 泛型擴展函式
date: 2025-09-16
keywords: kotlin, Extensions
---
在不變動原本的類別狀況下，寫擴展函式。

## String擴展函式
語法
```
類別.擴展函式名() : 傳回值類型 {
	內容
}
String.addExt() : 傳回值類型 {
	內容
}
```

### this
以下程式碼，this代表呼叫擴展函式的呼叫者。<br>
```
呼叫者.擴展函式()
"Hello".addExt()
```
「Hello」就是擴展函式的呼叫者，擴展函式都會自帶this指向擴展函式的呼叫者。

{% highlight kotlin linenos %}
fun String.addExt(): String {
    return this + ">>>>>>>>>>"
}

fun main() {
    println("Hello".addExt())
}
{% endhighlight %}
```
Hello>>>>>>>>>>
```

## 鏈式呼叫
另外寫了String.myprint()擴展函式。
{% highlight kotlin linenos %}
fun String.addExt(): String {
    return this + ">>>>>>>>>>"
}

fun String.myprint(): Unit {
    println(this.toString() + "@@@@@")
}

fun main() {
    "Hello".addExt().myprint()
}
{% endhighlight %}
```
Hello>>>>>>>>>>@@@@@
```

## 泛型擴展函式
任何類型都可以呼叫myprint()，要如何做？使用泛型擴展函式。<br>
```
fun <T> T.擴展函式()
```

以下改寫成泛型擴展函式，任意類型都可以使用myprint()。
{% highlight kotlin linenos %}
fun <T> T.myprint(): Unit {
    println(this.toString() + "@@@@@")
}

fun main() {
    123.myprint()
    true.myprint()
    "ABC".myprint()
}
{% endhighlight %}
```
123@@@@@
true@@@@@
ABC@@@@@
```
## T.let擴展函式

- [inline][1]

inline是內嵌函式，把函式包在呼叫它的函式程式碼中。<br>

T.let()任何類型都有let()擴展函式。
{% highlight kotlin linenos %}
public inline fun <T, R> T.let(block: (T) -> R): R {
    return block(this)
}
{% endhighlight %}

### 傳入參數給Lambda
以下程式碼block是參數名，參數類型為Lambda匿名函式，傳入(T)任意類型參數，傳回值是R任意類型。
{% highlight kotlin linenos %}
public inline fun <T, R> T.let(block: (T) -> R): R {
    return block(this)
}
{% endhighlight %}

block是一個Lambda匿名函式，函式傳入參數x類型為String，傳回類型為Unit。
{% highlight kotlin linenos %}
fun main() {
    val block: (String) -> Unit = { x -> print(x) }
    block("Hello world")
}
{% endhighlight %}
```
Hello world
```

Lambda只有一個參數，可省略x參數，使用it代替x，程式結果與上面執行相同。
{% highlight kotlin linenos %}
fun main() {
    val block: (String) -> Unit = { print(it) }
    block("Hello world")
}
{% endhighlight %}
```
Hello world
```

### T.let lambda
{% highlight kotlin linenos %}
public inline fun <T, R> T.let(block: (T) -> R): R {
    return block(this)
}
{% endhighlight %}

以下程式碼，花括號{}就是Lambda函式，it就是上面程式碼的this，this是擴展函式呼叫者，也就是\"Hello\"。
{% highlight kotlin linenos %}
"Hello".let {
    println(it)
}
{% endhighlight %}
```
Hello world
```

### T.let() 傳回值類型
1. 建立一個rtn變數接收Lambda傳回值。
2. 再將Lambda傳回值，作為傳回值傳回去。
{% highlight kotlin linenos %}
public inline fun <T, R> T.mylet(block: (T) -> R): R {
    // 1.建立一個rtn變數接收Lambda傳回值
    val rtn: R = block(this)
    // 2. 再將Lambda傳回值，作為傳回值傳回去。
    return rtn
}

fun main() {
    val rtn = "Hello World".mylet { println(it) }
    // 印出傳回值類型
    println(rtn)
}
{% endhighlight %}
```
Hello World
kotlin.Unit
```

## T.apply()
### T.()
apply()的Lambda參數類型為`T.() - > Unit`，其中的T.()是什麼？
{% highlight kotlin linenos %}
public inline fun <T> T.apply(block: T.() -> Unit): T {
    block()
    return this
}
{% endhighlight %}

先前提過，String.擴展函式()，以下的this是擴展函式呼叫者，\"Hello world\"。
{% highlight kotlin linenos %}
fun String.addExt(): String {
    return this + ">>>>>>>>>>"
}
fun main() {
    val str = "Hello world".addExt()
    println(str)
}
{% endhighlight %}
```
Hello world>>>>>>>>>>
```

所以這邊的T.()，可以視作T.擴展函式()，而這個T.就是呼叫apply()函式的呼叫者，會把呼叫者變成this傳入到T.擴展函式()中。<br>
而下面程式碼的this就是\"Hello world\"，這個字串。
{% highlight kotlin linenos %}
fun main() {
    "Hello world".apply { 
        println(this + "==========>>>>>>")
    }
}
{% endhighlight %}
```
Hello world==========>>>>>>
```

### this可以隱藏
下面程式碼this就是擴展函式呼叫者File。
{% highlight kotlin linenos %}
fun main() {
    File("/Users/cici/testc/file_test").apply {
        this.setWritable(true)
        this.setExecutable(true) //最後一行的結果才是傳回值
    }
}
{% endhighlight %}

也可以把this隱藏起來。
{% highlight kotlin linenos %}
fun main() {
    File("/Users/cici/testc/file_test").apply {
        setWritable(true)
        setExecutable(true) //最後一行的結果才是傳回值
    }
}
{% endhighlight %}

### T.apply() 傳回值類型
程式碼最後一行傳回的是this，也就是擴展函式呼叫者自己。
{% highlight kotlin linenos %}
public inline fun <T> T.apply(block: T.() -> Unit): T {
    block()
    return this
}
{% endhighlight %}

### 還原非泛型的程式碼
把上面程式碼的<T>刪除，T換成File，`public inline`刪掉，只剩下File.apply()，也就是File的擴展函式，Lambda參數也是代入File的擴展函式，會把擴展函式呼叫者this傳入。
{% highlight kotlin linenos %}
fun File.apply(block: File.() -> Unit): File {
    block()
    return this
}
{% endhighlight %}

## to infix
以下是建立一個map。
{% highlight kotlin linenos %}
val map1 = mapOf(
    "Alice" to 18,
    "Alex" to 20,
    "Momo" to 5,
    "Yoyo" to 8
)
{% endhighlight %}

to的擴展函式，使用到infix，infix可以把呼叫的點`.`變成空白。<br>
to的擴展函式傳回值為Pair<A,B>，A與B是泛型類型。<br>
{% highlight kotlin linenos %}
public infix fun <A, B> A.to(that: B): Pair<A, B> = Pair(this, that)
{% endhighlight %}

原本to的擴展函式程式碼:
{% highlight kotlin linenos %}
fun main() {
    val pair1 = "Alice".to(18)
}
{% endhighlight %}

infix可以把點`.`變空白，跟上面的程式碼是相同的。<br>
{% highlight kotlin linenos %}
fun main() {
    val pair1 = "Alice" to 18
}
{% endhighlight %}

## Int.until
Int.until是Int的擴展函式，fun前面有infix，代表可以使用空白呼叫。<br>

this是呼叫擴展函式的呼叫者，`2.until(7)`，2就是呼叫者，2就是this。<br>

to是參數，7就是to。<br>

to是參數，傳回值類型是IntRange`..`。<br>

傳回值 2 .. 7-1<br>
{% highlight kotlin linenos %}
public infix fun Int.until(to: Int): IntRange {
    if (to <= Int.MIN_VALUE) return IntRange.EMPTY
    return this .. (to - 1).toInt()
}
{% endhighlight %}

原本是
{% highlight kotlin linenos %}
2.until(7)
{% endhighlight %}

infix可以把點`.`變空白
{% highlight kotlin linenos %}
2 until 7
{% endhighlight %}

## String.count()擴展函式
String.count()擴展函式是計算字串有幾個字元。
{% highlight kotlin linenos %}
println("字元個數 = ${"Hello World".count()}")
{% endhighlight %}
```
字元個數 = 11
```

### count(Lambda參數)
使用有參數的count()擴展函式，參數為Lambda。
{% highlight kotlin linenos %}
public inline fun CharSequence.count(predicate: (Char) -> Boolean): Int {
    var count = 0
    // 遍歷字串每一個字元
    for (element in this) {
      // 如果Lambda傳回值是true，count計數器 + 1
      if (predicate(element)) ++count
    }
    return count
}
{% endhighlight %}

Lambda類型為參數是Char，傳回值是Boolean
{% highlight kotlin linenos %}
predicate: (Char) -> Boolean
{% endhighlight %}

自己寫一個Lambda，參數是letter，如果letter等於o，就傳回true
{% highlight kotlin linenos %}
{ letter ->
    letter == 'o'
}
{% endhighlight %}

把Lambda傳入String.count()擴展函式。
{% highlight kotlin linenos %}
var str = "Hello World"
val o_count = str.count ({ letter ->
    letter == 'o'
})
{% endhighlight %}

簡化，把圓括號去掉。
{% highlight kotlin linenos %}
var str = "Hello World"
val o_count = str.count { letter ->
    letter == 'o'
}
{% endhighlight %}

簡化，letter與->箭頭去掉，因為只有一個參數，可以去掉，默認用it代替一個參數。
{% highlight kotlin linenos %}
var str = "Hello World"
val o_count = str.count {
    it == 'o'
}
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
fun main() {
    var str = "Hello World"
    val o_count = str.count {
        it == 'o'
    }
    println("字元個數 = ${str.count()}")
    println("o字元個數 = ${o_count}")
}
{% endhighlight %}
```
字元個數 = 11
o字元個數 = 2
```

## 擴展屬性
除了擴展函式之外，可以為原有的類別，重新定義擴展屬性。<br>
下面程式碼，mylen是自訂屬性，使用`get() = count()`取得字串長度。<br>
{% highlight kotlin linenos %}
val String.mylen
    get() = count()

fun main() {
    println("字元個數 = ${"Hello World".mylen}")
}
{% endhighlight %}

[1]: {% link _pages/c/function/func_inline.md %}
