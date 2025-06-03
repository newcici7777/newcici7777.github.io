---
title: 繼承 覆寫 轉型
date: 2025-05-31
keywords: kotlin, extends
---
Prerequisites:

- [繼承父類別建構式][1]

## open
Kotlin預設是不能被繼承，若要讓類別可以被繼承，在class前面加上open。

在fun前面加上open，才能被子類別覆寫。

在變數前面加上open，才能被子類別覆寫。

{% highlight kotlin linenos %}
open class Parent {  // 使用open class
    open var name: String = ""  // 使用open 變數
    open fun showData() {}  // 使用open fun
}
{% endhighlight %}

## override
子類別 : 父類別()來繼承父類別。

覆寫父類別的方法，前面要加上override。

{% highlight kotlin linenos %}
open class Parent {  // 使用open class
    open var name: String = "Parent"  // 使用open 變數
    open fun showData() {}  // 使用open fun
}
{% endhighlight %}

{% highlight kotlin linenos %}
class Child : Parent() {
    override var name: String = "Child"
        set(value) {
            field = value.trim()
        }
    override fun showData() {  // 使用override fun
        println("name = $name")
    }
}
{% endhighlight %}

{% highlight kotlin linenos %}
fun main() {
    var child: Child = Child()
    child.showData()
    child.name = "   Alex   "
    child.showData()
}
{% endhighlight %}
```
name = Child
name = Alex
```

## 多型
多型就是類型是父類別，實際上指向子類別，執行時也是子類別。

{% highlight kotlin linenos %}
fun main() {
    var obj: Parent = Child()
    obj.showData()
    obj.name = "   Alex   "
    obj.showData()
}
{% endhighlight %}
```
name = Child
name = Alex
```

## 轉型as
在子類別增加一個方法showInfo，這個方法只有子類別才有。
{% highlight kotlin linenos %}
class Child : Parent() {
    override var name: String = "Child"
        set(value) {
            field = value.trim()
        }
    override fun showData() {  // 使用override fun
        println("name = $name")
    }
    
    fun showInfo() {
        println("這是子類別才有的方法")
    }
}
{% endhighlight %}

在多型的狀況下，類型是父類別，根本找不到showInfo()的方法。
{% highlight kotlin linenos %}
fun main() {
    var obj: Parent = Child()
    obj.showData()
    
    //以下無法執行
    obj.showInfo()
}
{% endhighlight %}

需要將類型是父類別，轉型成類型是子類別。
{% highlight kotlin linenos %}
fun main() {
    var obj: Parent = Child()
    obj.showData()

    //以下可以執行了
    (obj as Child).showInfo()
}
{% endhighlight %}
```
name = Child
這是子類別才有的方法
```

不可能每一次呼叫showInfo()都要轉型，kotlin支援只要執行過一次as，obj不用再轉型。
{% highlight kotlin linenos %}
fun main() {
    var obj: Parent = Child()
    obj.showData()

    //以下可以執行了
    (obj as Child).showInfo()
    obj.showInfo()
    obj.showInfo()
}
{% endhighlight %}
```
name = Child
這是子類別才有的方法
這是子類別才有的方法
```

[1]: {% link _pages/kotlin/extends_constructor.md %}