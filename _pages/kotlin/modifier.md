---
title: 存取修飾子
date: 2025-06-02
keywords: kotlin, modifier
---
## 四種存取修飾子
- public
- private
- protected
- internal 只能在同一個module中存取。

## 屬性
在屬性前面加上存取修飾子。
{% highlight kotlin linenos %}
class Cat4 {
    private var age:Int = 0
}
{% endhighlight %}

在set前面加上存取修飾子，代表不允許其它地方設值，只有本身的類別可以設值。
{% highlight kotlin linenos %}
class Cat4 {
    var name:String = ""
        private set(value) {
            field = value
        }
}
{% endhighlight %}

可簡化為:
{% highlight kotlin linenos %}
class Cat4 {
    var name:String = ""
        private set
}
{% endhighlight %}

## 方法
在fun前面加上存取修飾子。
{% highlight kotlin linenos %}
class Cat4 {
    protected fun run() {

    }
}
{% endhighlight %}

## 建構子
{% highlight kotlin linenos %}
open class Parent protected constructor (val name: String) {
}
{% endhighlight %}

## 類別
{% highlight kotlin linenos %}
internal open class Parent {
}
{% endhighlight %}