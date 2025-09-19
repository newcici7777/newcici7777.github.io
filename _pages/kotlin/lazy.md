---
title: lazy
date: 2025-06-03
keywords: kotlin, lazy
---
所謂的lazy是指，當要使用時才初始化變數的內容。

類似[靜態內部類別][1]，使用到了，才會建立類別。

語法
```
val 變數名 by lazy { ... }
```

程式碼
{% highlight kotlin linenos %}
class Test {
    // 使用的時候，才初始化變數
    val info by lazy { load() }
    private fun load(): String {
        println("load setting")
        return "infomation"
    }
}
{% endhighlight %}

測試程式
{% highlight kotlin linenos %}
fun main() {
    val test: Test = Test()
    // 暫停3秒
    Thread.sleep(3000)
    // 3秒後才使用這個變數，使用的時候才初始化變數內容
    println(test.info)
}
{% endhighlight %}
```
load setting
infomation
```

[1]: {% link _pages/java/static_inner.md %}#使用到靜態內部類別才會被建立