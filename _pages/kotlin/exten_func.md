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
## let擴展函式

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

### let lambda
以下程式碼，花括號{}就是Lambda函式，it就是參數。
{% highlight kotlin linenos %}
"Hello".let {
    println(it)
}
{% endhighlight %}





[1]: {% link _pages/c/function/func_inline.md %}
