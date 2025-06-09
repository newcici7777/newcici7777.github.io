---
title: Component 解構
date: 2025-06-09
keywords: kotlin, component, destructure
---
Kotlin這邊的解構跟C++的解構子完全不同。

這邊的解構是把物件拆成好幾個變數。

## 類別
語法
{% highlight kotlin linenos %}
class 類別 {
operator fun component1() = 屬性
operator fun component2() = 屬性
operator fun component3() = 屬性
operator fun component4() = 屬性
.
.
.
}
{% endhighlight %}

物件拆成好幾個變數。
{% highlight kotlin linenos %}
val (屬性1, 屬性2, 屬性3) = 物件
{% endhighlight %}

範例
{% highlight kotlin linenos %}
class Pig(val name:String, val age:Int, val weight:Float) {
    operator fun component1() = name
    operator fun component2() = age
    operator fun component3() = weight
}
{% endhighlight %}

測試
{% highlight kotlin linenos %}
fun main() {
    val (name, age, weight) = Pig("佩佩", 10, 20.5f)
    println("name = $name, age = $age, weight = $weight")
}
{% endhighlight %}
```
name = 佩佩, age = 10, weight = 20.5
```

### 底線\_
不想指派屬性值，可以用底線。
{% highlight kotlin linenos %}
fun main() {
    val (name, _ , weight) = Pig("佩佩", 10, 20.5f)
    println("name = $name,  weight = $weight")
}
{% endhighlight %}
```
name = 佩佩,  weight = 20.5
```

### 集合
集合也有支援解構，把集合拆成好幾個變數。
{% highlight kotlin linenos %}
fun main() {
    val (name1, name2, name3) = listOf<String>("Mary", "Bill", "Eva")
    println("name1 = $name1,  name2 = $name2, name3 = $name3")
}
{% endhighlight %}
```
name1 = Mary,  name2 = Bill, name3 = Eva
```

### Data Class
- [Data Class 解構][1]

Data Class自動就會寫Componet的語法，不用自己寫。


[1]: {% link _pages/kotlin/data_class.md %}#解構
