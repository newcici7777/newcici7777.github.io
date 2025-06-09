---
title: 建構式
date: 2025-05-21
keywords: kotlin, constructor
---
建構式跟Java建構子一樣，主要是用來建立物件。

建構式分為主要建構式(Primary constructor)與次要建構式(Secondary constructor)。

## 預設主要建構式
constructor()為主要建構式，與「class 類別名」同一行，放在「類別名」後面，預設建構式沒有參數。

語法
```
class 類別名 constructor() {
}
```

{% highlight kotlin linenos %}
class Dog constructor() {
	
}
{% endhighlight %}

Kotlin目的在簡化程式碼，若建構式沒有參數，也沒有權限修飾子，可以省略constructor()，Kotlin會自動產生預設建構式。

{% highlight kotlin linenos %}
class Dog {
	
}
{% endhighlight %}

若有參數，可以省略constructor的文字，只留下圓括號(參數)，代表它是主要建構式。

```
class 類別名 (參數) {
}
```

## 主要建構式
之前在[屬性][1]的文章中提到，val唯讀屬性，不會有set()方法。

可由主要建構式設定val屬性的值，在類別中也不用再次定義val屬性。

語法，參數可以為var或val，沒有限定只能val。
```
class 類別名 (val 屬性: 類型, var 屬性: 類型, ...) {
	// 省略類別屬性，移到主要建構式
}
```

原本程式碼
{% highlight kotlin linenos %}
class Cat {
    // 類別屬性
    val name = ""
}
{% endhighlight %}

改為主要建構式，注意！可以不用給初始值，但一定要有類型。

不給類型會有這個編譯錯誤，A type annotation is required on a value parameter。

這一部分跟[Java final 建構子初始值][3]的內容一樣。

{% highlight kotlin linenos %}
// 屬性要有類型
class Cat (val name: String) {

}
{% endhighlight %}

由「主要建構式」建立物件。
{% highlight kotlin linenos %}
fun main() {
    val cat = Cat("小咪")
    println(cat.name)
}
{% endhighlight %}
```
小咪
```

## 主要建構式自動產生set()、get()
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

## 主要建構式 + 所有屬性預設值
每一個屬性都有預設值，建立物件時，只要代入`屬性名 = 值`，就可以設定屬性的值。
{% highlight kotlin linenos %}
class Cat(
    val name: String = "",
    var age: Int = 0,
    var color: String = ""
) {
}

fun main() {
    // 主要建構式 屬性名 = 值
    val cat = Cat(name = "小咪")
    println(cat.name)
}
{% endhighlight %}
```
小咪
```
## 次要建構式
### 次要建構式使用方式
次要建構式參數數量，不能跟主要建構式參數數量一樣。

次要建構式無法使用val與var，不會自動產生set()、get()。

因為無法使用val與var，所以會由以下三種方式搭配次要建構式。

1. 主要建構式 + 初始化所有屬性 + 次要建構式。
2. 類別屬性 + 次要建構式
```
class 類別名 {
  // 類別屬性
  var 屬性名: 屬性類型 = 初始值
  val 屬性名: 屬性類型 = 初始值
}
```
3. 主要建構式屬性 + 類別屬性 + 次要建構式

Kotlin 的設計原則是：「能用簡單寫法就不要複雜化」！

### 類別屬性 + 次要建構式
次要建構式無法修改val的屬性。

若要透過次要建構式修改屬性，請設為var。

注意！次要建構式沒有val與var。

次要建構式由constructor(屬性\:類型)，注意！屬性一定要有類型。

語法:
```
class 類別名 {
    // 類別屬性
    var 屬性1: 類型 = 初始值
    var 屬性2: 類型 = 初始值

    // 次要建構式
    constructor(屬性1: 類型, 屬性2: 類型) {
    	this.屬性1 = 屬性1
    	this.屬性2 = 屬性2
    } 
}
```

範例如下:
{% highlight kotlin linenos %}
class Cat1 {
    // 類別屬性
    var name: String = ""
    var age = 0
    var color = ""

    // 次要建構式
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

### 主要建構式屬性 + 類別屬性 + 次要建構式
主要建構式中的屬性可為val。

次要建構式無法修改val的屬性。

次要建構式後面就要加上 : this(屬性)，先呼叫主要建構式。

this(屬性)後面不用有類型。

範例:
{% highlight kotlin linenos %}
// 主要建構式屬性 
class Cat2(val name: String) {
    // 類別屬性
    var age = 0
    var color = ""

    // 次要建構式
    constructor(name: String, age: Int) : this(name) {
        // name是val，不能透過次要建構式修改
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

### 主要建構式 + 初始化所有屬性 + 次要建構式
{% highlight kotlin linenos %}
// 主要建構式 + 初始化所有屬性
class Cat3(
    val name: String = "",
    var age: Int = 0,
    var color: String = ""
) {
    // 次要建構式
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

## init初始化函式

- [Java匿名程式碼區塊][2]

這部分的知識跟Java匿名程式碼區塊很像，但執行順序不同，Java是先執行程式碼區塊，再執行建構子。

Kotlin是先執行主要建構式 -> 類別屬性 -> init初始化函式 -> 次要建構式。

以下這行不能編譯成功，Property must be initialized or be abstract

屬性必須要初始化。
{% highlight kotlin linenos %}
class Dog {
    val name: String
}
{% endhighlight %}

使用init初始化函式，類別中的屬性可以不用給初始值，由init函式初始化屬性。

這一部分跟[Java final 匿名區塊初始化][3]的內容一樣。

{% highlight kotlin linenos %}
class Dog1 {
    val name: String

    init {
    	// 由init函式初始化屬性
        name = "小白"
        loadDB()
    }
    fun loadDB() {
        println("DB loading")
        println("name = $name")
    }
}
fun main() {
    val dog1 = Dog1()
}
{% endhighlight %}
```
DB loading
name = 小白
```

## 主要建構式不用有val或var
主要建構式可以不用有val或var，沒有val與var的參數就不是類別屬性，會使用\_底線在參數前面，代表只使用一次。

{% highlight kotlin linenos %}
class Cat4 (_name: String, _age: Int) {
    var name:String = _name
    var age:Int = _age
}
fun main() {
    val cat4 = Cat4("喵喵", 2)
    println(cat4.name)
    println(cat4.age)
}
{% endhighlight %}
```
喵喵
2
```

### 可以與init搭配
{% highlight kotlin linenos %}
class Cat4 (_name: String, _age: Int) {
    var name:String = ""
    var age:Int = 0
    init {
        name = _name
        age = _age
    }
}
fun main() {
    val cat4 = Cat4("喵喵", 2)
    println(cat4.name)
    println(cat4.age)
|
{% endhighlight %}
```
喵喵
2
```

## 程式碼執行順序
1. 主要建構式
2. 類別屬性
3. init初始化函式
4. 次要建構式

### 使用Decompile證明執行順序
Kotlin程式碼
{% highlight kotlin linenos %}
class Cat4 (val name: String, _age: Int) { // 1. name
    private var age:Int = _age  //2.
    private val hobby:String
    private var sleeptimes: Int = 0
    init {
        println("init block ...")  // 3.
        hobby = "swimming"         // 4.
    }
    constructor(name: String, _age: Int, sleeptimes: Int):this(name, _age) {
        this.sleeptimes = sleeptimes  // 5.
    }
}
{% endhighlight %}

Java程式碼，以下已把建構子中的順序與Kotlin對映。
{% highlight java linenos %}
public final class Cat4 {
   private int age;
   private final String hobby;
   private int sleeptimes;
   @NotNull
   private final String name;

   @NotNull
   public final String getName() {
      return this.name;
   }
   public Cat4(@NotNull String name, int _age) {
      Intrinsics.checkNotNullParameter(name, "name");
      super();
      this.name = name;  // 1.
      this.age = _age;   // 2.
      String var3 = "init block ...";  // 3.
      System.out.println(var3);        // 3.
      this.hobby = "swimming";         // 4.
   }

   public Cat4(@NotNull String name, int _age, int sleeptimes) {
      Intrinsics.checkNotNullParameter(name, "name");
      this(name, _age);  // 先呼叫2個參數的父類建構子
      this.sleeptimes = sleeptimes;  //5.
   }
{% endhighlight %}

### 屬性不能寫在後面
你不能像C++把屬性寫到最後面，會無法讀取到屬性，Kotlin執行順序是由上而下。
{% highlight kotlin linenos %}
class Dog1 {
    init {
        loadDB()
    }

    fun loadDB() {
        println("DB loading")
        // 無法讀取到屬性
        println("name = $name")
    }

    // 屬性寫到最後面
    val name: String = "小白"
}
{% endhighlight %}
```
DB loading
name = null
```

[1]: {% link _pages/kotlin/field.md %}
[2]: {% link _pages/java/constructor.md %}
[3]: {% link _pages/java/final.md %}#不是static的final