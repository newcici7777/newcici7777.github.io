---
title: Exception
date: 2025-05-20
keywords: kotlin, Exception
---
Prerequisites:

- [Java 例外][1]

## try catch
{% highlight kotlin linenos %}
var str: String? = null
try {
    println(str!!.length)
} catch (e: KotlinNullPointerException) {
    e.printStackTrace()
}
{% endhighlight %}
```
Exception in thread "main" java.lang.NullPointerException
	at LearnNullKt.testNull(LearnNull.kt:53)
	at LearnNullKt.main(LearnNull.kt:62)
	at LearnNullKt.main(LearnNull.kt)
```

## 自訂例外

Prerequisites:

- [Java 自訂例外][1]

{% highlight kotlin linenos %}
class MyNullException : RuntimeException("空值")

fun checkNull(str: String?) {
    str ?: throw MyNullException()
}

fun main() {
    var str: String? = null
    try {
        checkNull(str)
    } catch (e: Exception) {
       println(e)
    }
}
{% endhighlight %}
```
MyNullException: 空值
```

## precondition functions
以下是kotlin提供的檢查值為null或值為false，所拋出的異常。

|函式    |檢查項目   |Exception thrown|
|:-----:|:----|:-----|
|checkNotNull| null |  IllegalStateException|
|require     | false| IllegalArgumentException|
|requireNotNull|null|IllegalStateException|
|error       | null |  IllegalStateException|
|assert      | false| AssertError|

{% highlight kotlin linenos %}
var str: String? = null
try {
    checkNotNull(str) {"空值"}
} catch (e: Exception) {
   println(e)
}
{% endhighlight %}
```
java.lang.IllegalStateException: 空值
```

[1]: {% link _pages/java/exception.md %}
[2]: {% link _pages/java/exception.md %}#自訂例外