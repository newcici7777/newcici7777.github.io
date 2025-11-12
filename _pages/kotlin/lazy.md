---
title: by lazy 延遲初始化
date: 2025-06-03
keywords: kotlin, Lazy Initialization
---
延遲初始化（Lazy Initialization）

語法
```
val 變數 by lazy { 
    ... 

    傳回值
}
```
- 傳回值是設定變數的內容。

所謂的lazy是指，當要使用時才初始化變數的內容，而且只會初始化一次，記憶體就會記錄變數的值。

{% highlight kotlin linenos %}
val lazyValue:String by lazy {
    println("設值並初始化")
    "Hello"
}
fun main() {
    println(lazyValue)
    println(lazyValue)
}
{% endhighlight %}
```
設值並初始化
Hello
Hello
```

以上的程式碼，讀取 lazyValue 變數二次，但只有第一次有執行`println("設值並初始化")`，接下來把`Hello`儲存在lazyValue的變數中，變數是建立在記憶體中的某一個空間。<br>

第二次使用的時候，直接讀取記憶體中存放的值。<br>

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

以下程式碼，建立Test物件時，並不會初始化info 屬性，直到使用變數，才呼叫load()函式，並傳回infomation，把info屬性的值設為infomation。<br>
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

lazy的概念有點像Java的[靜態內部類別][1]

[1]: {% link _pages/java/static_inner.md %}#使用到靜態內部類別才會被建立