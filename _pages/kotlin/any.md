---
title: Any
date: 2025-05-30
keywords: kotlin, Any
---
Prerequisites:

- [Java ==與equals][1]

Any是所有類別的父類別。

## ===與==
Any===與==都是比較記憶體位址是否相同。

class繼承Any若沒有覆寫equals的狀況下，==與===的功能是相同的，都是比較記憶體位址。

如同Java的Object，equals()的功能與==是相同的，都是比較記憶體位址是否相同。

obj1與obj2為二個物件。
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane()
    val obj2 = Airplane()
    println("equals = ${obj1 == obj2}")
    println("compare = ${obj1 === obj2}")
}
class Airplane {
}
{% endhighlight %}
```
equals false
compare false
```

將obj2指向obj1記憶體位址。
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane()
    val obj2 = obj1
    println("equals = ${obj1 == obj2}")
    println("compare = ${obj1 === obj2}")
}
class Airplane {
}
{% endhighlight %}
```
equals = true
compare = true
```

執行結果==與===都是比較記憶體位址是否相同。

## toString()
印出package\+class name@16進制的hashCode
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane(1, 500)
    println(obj1.toString())
}
class Airplane(val id:Int, val capacity:Int = 0) {
}
{% endhighlight %}
```
Airplane@5674cd4d
```

印出物件，也是呼叫toString()的方法，以下程式碼的結果跟上面的一樣。
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane(1, 500)
    println(obj1)
}
class Airplane(val id:Int, val capacity:Int = 0) {
}
{% endhighlight %}
```
Airplane@5674cd4d
```

## hashCode
使用記憶體位址進行運算，每個物件的hashCode是不相同的。
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane(1, 500)
    println(obj1.hashCode())
}
class Airplane(val id:Int, val capacity:Int = 0) {
}
{% endhighlight %}
```
1450495309
```




[1]: {% link _pages/java/equals_compare.md %}