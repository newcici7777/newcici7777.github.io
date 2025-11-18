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

{% highlight kotlin linenos %}
var count = 10

fun main() {
    val prop = ::count   // 屬性引用
    println(prop.get())  // 10
    prop.set(20)
    println(count)       // 20
}
{% endhighlight %}

::count 是 屬性引用 (Property Reference)

它不是直接取得 count 的值，而是 一個物件，類型是：
```
KMutableProperty0<Int>
```
KMutableProperty0物件本身提供 get() 與 set(value) 方法，用來操作原始變數 count。

### prop.get()
```
println(prop.get())
```
prop 代表 count 的屬性引用

.get() 呼叫時，實際上是去 讀取 count 目前的值

因為 count = 10，所以印出 10

### prop.set(20)
```
prop.set(20)
```
.set(value) 會去 改變原本的 count 值

等於 count = 20

### 最後 println(count)
```
println(count)
```
因為前面用 prop.set(20) 改了值

所以 count 現在變成 20

印出 20

### 重要觀念

::count → 屬性引用物件（Property Reference）

prop.get() → 取得原變數值（getter）

prop.set(value) → 改變原變數值（setter）

好處：

可以把變數當成「可操作的物件」傳遞或存放

可以動態操作屬性（像委託、泛型函式、reflection）

把這個屬性當作「物件」傳給別人使用

不會馬上執行 取值（對屬性），只是拿它的「描述」或「參考」

## `::`屬性的種類
### 全域變數：
```
::topLevelProperty
```
### 成員變數：
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

### 屬性傳遞給Kotlin標準庫使用
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