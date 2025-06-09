---
title: Data class
date: 2025-05-30
keywords: kotlin, Data class
---
Prerequisites:

- [Any][1]
- [Java ==與equals][2]

## ==與===
一般類別繼承Any，所以==與===都是比較記憶體位址是否相同。

Data class資料類別會自動覆寫equals()，
所以==是比較內容是否相同，===是比較記憶體位址是否相同。

{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane(1, 500)
    val obj2 = Airplane(1, 500)
    println("equals = ${obj1 == obj2}")
    println("compare = ${obj1 === obj2}")
}
data class Airplane(val id:Int, val capacity:Int = 0) {
}
{% endhighlight %}
```
equals = true
compare = false
```
## toString()
Data class資料類別會自動覆寫toString()。

{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane(1, 500)
    println(obj1.toString())
}
data class Airplane(val id:Int, val capacity:Int = 0) {
}
{% endhighlight %}
```
Airplane(id=1, capacity=500)
```

## copy() 拷貝
使用copy()拷貝物件，參數可以是要修改的屬性。
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane(1, 500)
    // 參數可以為要修改的屬性
    val obj2 = obj1.copy(id = 10)
    println(obj2)
}
data class Airplane(val id:Int, val capacity:Int = 0) {
}
{% endhighlight %}
```
Airplane(id=10, capacity=500)
```

### copy()不支援次要建構式
次要建構式多了weight屬性。
{% highlight kotlin linenos %}
data class Airplane(val id: Int, val capacity: Int = 0) {
    var weight: Int = 0;

    constructor(id: Int, capacity: Int, weight: Int) : this(id, capacity) {
        this.weight = weight
    }

    override fun toString(): String {
        return "Airplane(id=$id, capacity=$capacity, weight=$weight)"
    }
}
{% endhighlight %}

使用次要建構式測試。
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Airplane(1, 500, 100)
    println(obj1)
    val obj2 = obj1.copy(id = 10)
    println(obj2)
}
{% endhighlight %}
```
Airplane(id=1, capacity=500, weight=100)
Airplane(id=10, capacity=500, weight=0)
```
由執行結果可以發現，obj2的weight仍是0，沒有跟著copy過來。

## 解構
[Component 解構][3]

Data Class自動就會寫Componet的語法，不用自己寫。

{% highlight kotlin linenos %}
fun main() {
    val(id, capacity) = Airplane(7, 500)
    println("id = $id, capacity = $capacity")
}
{% endhighlight %}
```
id = 7, capacity = 500
```

[1]: {% link _pages/kotlin/any.md %}
[2]: {% link _pages/java/equals_compare.md %}
[3]: {% link _pages/kotlin/component.md %}