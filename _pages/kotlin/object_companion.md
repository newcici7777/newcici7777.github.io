---
title: object與companion object
date: 2025-06-03
keywords: kotlin, object, singleton, companion object
---
Prerequisites:

- [Singleton][1]
- [靜態內部類別][2]

## object 類別
這裡的object，指的是就只有一個物件，不管被建立幾次，只會被建立一次。

[Singleton][1]的概念。

語法
```
object 類別名 {

}
```

使用方法
```
類別名.方法()
類別名.屬性名
```

建立object物件
{% highlight kotlin linenos %}
object Single {
    val name:String = "Single"
    fun method1() {
        println("method1")
    }
    fun method2() {
        println("method2")
    }
}
{% endhighlight %}

測試
{% highlight kotlin linenos %}
fun main() {
    Single.method1()
    Single.method2()
    println(Single.name)
    println(Single.hashCode())
    println(Single.hashCode())
    println(Single.hashCode())
}
{% endhighlight %}
```
method1
method2
Single
1450495309
1450495309
1450495309
```

hashCode如同物件身份證，代表印出三次Single物件，hashCode都相同，代表同一個物件。

## companion object
跟[靜態內部類別][3]一樣，用到的時候才會被載入。
```
class 外部類別 {
	companion object {
		變數
		方法
	}
}
```

程式碼
{% highlight kotlin linenos %}
class Outter {
    init {
        println("建立外部類別")
    }
    companion object {
        val info: String = "info"
        init {
            println("建立內部類別")
        }
        fun load() {
            println("載入資訊...")
        }
    }
}
{% endhighlight %}

只能透過類別名，才能呼叫companion object建立的變數與方法。
```
類別名.方法
類別名.變數
```

以下會編譯錯誤。
{% highlight kotlin linenos %}
fun main() {
    val obj = Outter()
    // 以下編譯錯誤
    obj.info
    obj.load()
}
{% endhighlight %}

透過外部類別名，才能取得方法與變數名。
{% highlight kotlin linenos %}
fun main() {
    Outter.load()
    println(Outter.info)
}
{% endhighlight %}
```
載入資訊...
info
```

[1]: {% link _pages/design_pattern/singleton.md %}
[2]: {% link _pages/java/static_inner.md %}
[3]: {% link _pages/java/static_inner.md %}#使用到靜態內部類別才會被建立