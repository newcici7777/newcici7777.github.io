---
title: 繼承與覆寫
date: 2025-05-31
keywords: kotlin, extends
---
## open
Kotlin預設是不能被繼承，若要讓類別可以被繼承，在class前面加上open，可以被子類別覆寫的方法，在fun前面加上open，才能被子類別覆寫。

{% highlight kotlin linenos %}
open class Parent {  // 使用open class
    open fun showData() {}  // 使用open fun
}
{% endhighlight %}

## override
用子類別 : 父類別()來繼承父類別。

覆寫父類別的方法，前面要加上override。
{% highlight kotlin linenos %}
open class Parent {
    open fun showData() {}
}
class Child : Parent() {
    override fun showData() {  // 使用override fun
        println("這是覆寫showData")
    }
}
fun main() {
    val obj2: Parent = Child()
    obj2.showData()
}
{% endhighlight %}
```
這是覆寫showData
```