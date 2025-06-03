---
title: 內部類別
date: 2025-06-03
keywords: kotlin, inner class
---
Prerequisites:

- [Java內部類別][1]

語法
```
外部類別{
	內部類別{

	}
}
```

{% highlight kotlin linenos %}
class Outter2 {
    class Inner(val name: String) {
        fun show() {
            println("inner class name = $name")
        }
    }
}
{% endhighlight %}

使用方法
```
外部類別名.內部類別名(主要建構式).方法()
```

{% highlight kotlin linenos %}
fun main() {
    Outter2.Inner("Mary").show()
}
{% endhighlight %}
```
inner class name = Mary
```

[1]: {% link _pages/java/innerclass.md %}