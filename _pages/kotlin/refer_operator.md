---
title: 二個冒號引用
date: 2025-11-18
keywords: kotlin, member reference operator
---
## 取得KClass
java寫法，取得Class
```
類別名.class
```

kotlin 取得KClass
```
類別名::class
```

kotlin 取得Java Class
```
類別名::class.java
```

## 屬性引用（Property Reference)

把這個屬性當作物件傳給別人使用

不會馬上執行 取值（對屬性），只是拿它的「描述」或「參考」

全域變數：
```
::topLevelProperty
```
成員變數：
```
ClassName::property 或 obj::property
```
可以用 .get() / .set()

{% highlight kotlin linenos %}
var count = 10

fun main() {
    val prop = ::count   // 全域變數引用
    println(prop.get())  // 10
    prop.set(20)
    println(count)       // 20
}
{% endhighlight %}

屬性傳遞給Kotlin標準庫使用。
{% highlight kotlin linenos %}
val names = listOf("Alice", "Bob")
val lengths = names.map(String::length)  // 取得 String.length 屬性引用
println(lengths)  // [5, 3]
{% endhighlight %}

## 函式引用
函式引用（Function Reference）

將函式當作物件傳遞，不立即執行

語法：
```
::函式名
```
{% highlight kotlin linenos %}
fun greet(name: String) = println("Hello, $name")

fun main() {
    val f: (String) -> Unit = ::greet  // 取得 greet 的函式引用
    f("Mia")  // Hello, Mia
}
{% endhighlight %}

作為參數，傳遞「函式」。
{% highlight kotlin linenos %}
val names = listOf("Alice", "Bob")
names.forEach(::println)   // 使用 println 的函式引用
{% endhighlight %}

## 成員函式引用（Member Function Reference）

對某個物件的成員函式做引用

語法：
```
物件::函式 或 類別::函式
```

{% highlight kotlin linenos %}
class Greeter {
    fun sayHi(name: String) = println("Hi, $name")
}

fun main() {
    val greeter = Greeter()
    val f = greeter::sayHi  // 物件成員函式引用
    f("Mia")  // Hi, Mia
}
{% endhighlight %}

{% highlight kotlin linenos %}
val f2: Greeter.(String) -> Unit = Greeter::sayHi
val g = Greeter()
g.f2("Amy")
{% endhighlight %}

## 小結

|用法|	說明|	範例|
|函式引用|	將函式當物件|	::greet|
|成員函式引用|	對物件成員函式|	obj::sayHi|
|屬性引用|	拿屬性當物件	|::count、obj::prop|
|建構子引用|	將 constructor 當函式	|::Person|
|高階函式參數|	傳給 map, sortedBy 等|	list.map(String::length)|