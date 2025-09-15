---
title: in out 泛型轉型
date: 2025-09-12
keywords: kotlin, in out
---
## out
泛型類型只能作為方法傳回值。<br>

泛型類型前面加上out。
```
class 類別名<out T>
```

下面建構子是private，代表建立物件要設初始值，不能被更改。<br>
{% highlight kotlin linenos %}
class Producer<out T>(private val item: T) {
    fun get(): T {
        return item
    }
}
{% endhighlight %}

## in
泛型類型只能作為方法的參數。<br>

泛型類型前面加上in。
```
class 類別名<in T>
```

{% highlight kotlin linenos %}
class Consumer<in T> {
    fun consume(item: T) {
        println("消費 $item")
    }
}
{% endhighlight %}

## 轉型
{% highlight kotlin linenos %}
class Producer<out T>(private val item: T) {
    fun get(): T {
        return item
    }
}

open class Food(val name: String)
class Burger(name: String) : Food(name)

class Consumer<in T> {
    fun consume(item: T) {
        println("消費 $item")
    }
}

fun main() {
    // 子類泛型轉父類泛型
    val foodProducer:Producer<Food> = Producer<Burger>(Burger("大享堡"))
    // 父類泛型
    val foodConsumer:Consumer<Food> = Consumer()
    // 父類泛型轉子類泛型
    val burgerConsumer:Consumer<Burger> = foodConsumer
}
{% endhighlight %}