---
title: object 匿名類別
date: 2025-06-03
keywords: kotlin, object, anonymous
---
Prerequisites:

- [Java匿名類別][1]

什麼是匿名類別？就是子類別沒有名字，由父類別或介面、或抽象類別的名字來建立的子類別。

語法
```
object : 類別 或 介面 或 抽象類別(建構子) {
	子類別的內容
     ....覆寫變數....
     ....覆寫方法....
}
```

{% highlight kotlin linenos %}
fun main() {
    // 建立Parent的子類別
    val obj1 = object: Parent() {
        override fun showData() {
            println("匿名類別的方法")
        }
    }
    obj1.showData()
}
open class Parent {  // 使用open class
    open var name: String = "Parent"  // 使用open 變數
    open fun showData() {}  // 使用open fun
}
{% endhighlight %}
```
匿名類別的方法
```

[1]: {% link _pages/java/anonymous_class.md %}