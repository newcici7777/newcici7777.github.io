---
title: 建構函式
date: 2025-05-21
keywords: kotlin, constructor
---
建構函式跟Java建構子一樣，主要是用來建立物件。

建構函式分為主要建構函式(Primary constructor)與次要建構函式(Secondary constructor)。

## 預設主要建構函式
constructor()為主要建構函式，與class Student同一行，放在後面，預設建構函式沒有參數。

語法
```
class 類別名 constructor() {
}
```

{% highlight kotlin linenos %}
class Dog constructor() {
	
}
{% endhighlight %}

Kotlin目的在簡化程式碼，若建構函式沒有參數，可以省略constructor()，Kotlin會自動產生預設建構函式。

{% highlight kotlin linenos %}
class Dog {
	
}
{% endhighlight %}

若有參數，可以省略constructor的文字，只留下圓括號(參數)，代表它是主要建構式。

```
class 類別名 (參數) {
}
```

## 主要建構函式
之前在[屬性][1]的文章中提到，val唯讀屬性，不會有set()方法。

可由建構函式設定val屬性的值，在類別主體body也不用再次定義val屬性。

語法，參數可以為var或val，沒有限定只能val。
```
class 類別名 (val 屬性: 類型, var 屬性: 類型, ...) {
}
```

原本程式碼
{% highlight kotlin linenos %}
class Cat {
    val name = ""
}
{% endhighlight %}

改為主要建構函式，注意！可以不用給初始值，但一定要有類型。

不給類型會有這個編譯錯誤，A type annotation is required on a value parameter。

{% highlight kotlin linenos %}
// 屬性要有類型
class Cat (val name: String) {

}
{% endhighlight %}

由「主要建構函式」建立物件。
{% highlight kotlin linenos %}
fun main() {
    val cat = Cat("小咪")
    println(cat.name)
}
{% endhighlight %}
```
小咪
```

## 主要建構函式自動產生set()、get()
搭配val與var，會自動產生set()、get()，注意！val不會自動產生set()方法。

因為val是唯讀，不能修改。

Java原始碼，因為val name唯讀，所以只有getName()，沒有setName()。
{% highlight java linenos %}
public final class Cat {
   @NotNull
   private final String name;

   @NotNull
   public final String getName() {
      return this.name;
   }

   public Cat(@NotNull String name) {
      Intrinsics.checkNotNullParameter(name, "name");
      super();
      this.name = name;
   }
}
{% endhighlight %}

## 主要建構函式 + 所有屬性預設值
每一個屬性都有預設值，建立物件時，只要代入`屬性名 = 值`，就可以設定屬性的值。
{% highlight kotlin linenos %}
class Cat(
    val name: String = "",
    var age: Int = 0,
    var color: String = ""
) {
}

fun main() {
    // 主要建構函式 屬性名 = 值
    val cat = Cat(name = "小咪")
    println(cat.name)
}
{% endhighlight %}
```
小咪
```
## 次要建構函式
### 次要建構函式使用方式
次要建構函式無法使用val與var，不會自動產生set()、get()。

因為無法使用val與var，所以會由以下三種方式搭配次要建構函式。

1. 主要建構函式 + 初始化所有屬性 + 次要建構函式。
2. 類別主體屬性 + 次要建構函式
```
class 類別名 {
  // 類別主體body屬性
  var 屬性名: 屬性類型 = 初始值
  val 屬性名: 屬性類型 = 初始值
}
```
3. 主要建構函式屬性 + 類別主體屬性 + 次要建構函式

Kotlin 的設計原則是：「能用簡單寫法就不要複雜化」！

### 類別主體屬性 + 次要建構函式
次要建構函式無法修改val的屬性。

若要透過次要建構函式修改屬性，請設為var。

注意！次要建構函式沒有val與var。

次要建構函式由constructor(屬性\:類型)，注意！屬性一定要有類型。

語法:
{% highlight kotlin linenos %}
class 類別名 {
    // 類別主體屬性
    var 屬性1: 類型 = 初始值
    var 屬性2: 類型 = 初始值

    // 次要建構函式
    constructor(屬性1: 類型, 屬性2: 類型) {
    	this.屬性1 = 屬性1
    	this.屬性2 = 屬性2
    } 
}
{% endhighlight %}

範例如下:
{% highlight kotlin linenos %}
class Cat1 {
    // 類別主體body定義屬性
    var name: String = ""
    var age = 0
    var color = ""

    // 次要建構函式
    constructor(name: String, age: Int) {
        this.name = name
        this.age = age
    }
}
{% endhighlight %}

測試如下:
{% highlight kotlin linenos %}
fun main() {
	val cat1 = Cat1("小咪", 10)
	println(cat1.name)
	println(cat1.age)
}
{% endhighlight %}
```
小咪
10
```

### 主要建構函式屬性 + 類別主體屬性 + 次要建構函式
主要建構函式中的屬性可為val。

次要建構函式無法修改val的屬性。

次要建構函式後面就要加上 : this(屬性)，先呼叫主要建構式。

this(屬性)後面不用有類型。

範例:
{% highlight kotlin linenos %}
// 主要建構函式屬性 
class Cat2(val name: String) {
    // 類別主體屬性
    var age = 0
    var color = ""

    // 次要建構函式
    constructor(name: String, age: Int) : this(name) {
        // name是val，不能透過次要建構函式修改
        // 注意！此處沒有this.name = name
        this.age = age
    }
}
{% endhighlight %}

測試:
{% highlight kotlin linenos %}
val cat2 = Cat2("喵喵", 2)
println(cat2.name)
println(cat2.age)
{% endhighlight %}
```
喵喵
2
```

### 主要建構函式 + 初始化所有屬性 + 次要建構函式
{% highlight kotlin linenos %}
// 主要建構函式 + 初始化所有屬性
class Cat3(
    val name: String = "",
    var age: Int = 0,
    var color: String = ""
) {
    // 次要建構函式
    constructor(name: String, age: Int) : this(name) {
        this.age = age
    }
}
{% endhighlight %}

測試
{% highlight kotlin linenos %}
val cat3 = Cat3("嘟嘟", 10)
println(cat3.name)
println(cat3.age)
{% endhighlight %}
```
嘟嘟
10
```
### 屬性初始化順序
1. 主要建構函式參數
2. 類別body屬性
3. 次要建構函式

### 個人觀感
以上所有方式，只有主要建構函式 + 所有屬性預設值，這個方法最優。


[1]: {% link _pages/kotlin/field.md %}
