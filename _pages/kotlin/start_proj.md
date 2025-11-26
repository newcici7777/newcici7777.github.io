---
title: 星號投射（star-projection）
date: 2025-11-18
keywords: kotlin, star-projection
---
Prerequisites:

- [Class][1]
- [反射][2]
- [二個冒號::引用][3]
- [泛型][4]
- [object_companion][5]
- [by lazy][6]
- [by][7]
- [Java ? extends Object][8]

KProperty<T> 是什麼？

在 Kotlin reflection（反射）裡，KProperty<T> 代表一個屬性（property）的描述物件

T 是屬性的型別，例如：
{% highlight kotlin linenos %}
val count: Int = 10
val prop: KProperty<Int> = ::count
{% endhighlight %}

這裡 T = Int。

為什麼會寫 `KProperty<*>`？

\* 在 Kotlin 泛型裡叫 星號投射（star-projection）

意思是「不管這個泛型實際是什麼類型，我都可以接受」

類似於 Java 的 ? 或 ? extends Object（上界 wildcard）

範例
{% highlight kotlin linenos %}
fun printName(prop: KProperty<*>) {
    println("屬性名稱: ${prop.name}")
}
{% endhighlight %}

無論是 KProperty<Int>、KProperty<String>，都可以傳入

因為我們只想拿 name，不關心具體型別

Kotlin 的 \* vs Java 的 ?

|Kotlin|	Java|	說明|
|`KProperty<*>`|	KProperty<?>|	可以接任何型別，等同於 Java wildcard|
|KProperty<out Any?>|	KProperty<? extends Object>|	上界通配符|
|KProperty<in String>|	KProperty<? super String>|	下界通配符|

所以 \* 不是「所有類別」本身，而是「不管泛型是什麼都接受」

實務例子
{% highlight kotlin linenos %}
var name: String = "Mia"
var age: Int = 30

fun printPropertyName(prop: KProperty<*>) {
    println(prop.name)
}

fun main() {
    printPropertyName(::name) // 可以接受 String
    printPropertyName(::age)  // 可以接受 Int
}
{% endhighlight %}

如果寫成 KProperty<String> 就不能傳 ::age

用 `KProperty<*>` 就能同時接受任意型別屬性

總結：

`KProperty<*>` = 「不管泛型型別是什麼都可以」

對應 Java 的 KProperty<?>

常用在只需要屬性描述（如 name、get()、set()）而不關心型別時。

[1]: {% link _pages/java/class.md %}
[2]: {% link _pages/kotlin/kotlin_reflect.md %}
[3]: {% link _pages/kotlin/refer_operator.md %}
[4]: {% link _pages/kotlin/generics.md %}
[5]: {% link _pages/kotlin/object_companion.md %}
[6]: {% link _pages/kotlin/lazy.md %}
[7]: {% link _pages/kotlin/delegate.md %}
[8]: {% link _pages/java/generics_extend_super.md %}