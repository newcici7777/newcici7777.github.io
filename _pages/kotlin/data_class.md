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


[1]: {% link _pages/kotlin/any.md %}
[2]: {% link _pages/java/equals_compare.md %}