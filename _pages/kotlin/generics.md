---
title: 泛型類別與泛型方法
date: 2025-09-12
keywords: kotlin, Generics
---
Prerequisites:

- [Java泛型類別][1]
- [Java泛型方法][2]
- [Kotlin vararg可變參數][3]

先了解以上文章，才能進行下面的文章內容。

## 泛型類別
語法
```
class 類別名<T> {}
```
T是類型，不知道是什麼類型。<br>

泛型可以用在屬性、方法、也可以用在建構式。<br>

本範例是使用在主建構式，T類型可以是任意類型，把T類型的參數，指派給obj成員屬性。<br>

{% highlight kotlin linenos %}
class Obj<T>(val obj: T) {
    fun print() {
        println(obj.toString())
    }
}
{% endhighlight %}

以下有三個類別，分別是男孩、男人、老鼠，使用data class是因為data class自動會產生toString()方法，toString()不用自己寫。<br>
{% highlight kotlin linenos %}
data class Boy(val name:String, val age:Int)
data class Man(val name: String, val age:Int)
data class Mouse(val name:String, val age:Int, val weight:Int)
{% endhighlight %}

### 使用泛型類別語法
```
類別<類型>(物件)
Obj<Boy>(Boy("Bill", 5))
```
若尖括號的是Boy，建構式()就不能傳入其它類別，如:Mouse，要彼此對映。

使用Obj(物件)主要建構式，把物件設給obj成員屬性。<br>
可以發現T類型可以接收任意類型的物件。<br>
{% highlight kotlin linenos %}
fun main() {
    val obj1 = Obj<Boy>(Boy("Bill", 5))
    val obj2 = Obj<Man>(Man("Jack", 30))
    val obj3 = Obj<Mouse>(Mouse("Kiki",1, 1))
    obj1.print()
    obj2.print()
    obj3.print()
}
{% endhighlight %}
```
Boy(name=Bill, age=5)
Man(name=Jack, age=30)
Mouse(name=Kiki, age=1, weight=1)
```

在Java宣告List的泛型，左邊類型已經使用尖括號<String>，右邊的尖括號<>中就不會寫類型。<br>
{% highlight java linenos %}
ArrayList<String> list = new ArrayList<>();
{% endhighlight %}

Kotlin也是一樣，等號左邊已經宣告類型`obj1: Obj<Boy>`，等號右邊就不用有`= Obj<Boy>()`
{% highlight kotlin linenos %}
 val obj1: Obj<Boy> = Obj(Boy("Bill", 5))
{% endhighlight %}

### 泛型約束
建立一個Human類別
{% highlight kotlin linenos %}
open class Human
{% endhighlight %}

以下Boy與Man類別繼承Human
{% highlight kotlin linenos %}
data class Boy(val name: String, val age: Int) : Human()
data class Man(val name: String, val age: Int) : Human()
{% endhighlight %}

T泛型類型後面加上`: Human`，代表類型只能是Human的子類才能傳入主要建構式。
{% highlight kotlin linenos %}
class Obj<T : Human>(val obj: T)
{% endhighlight %}

完整程式碼:以下`val obj3: Obj<Mouse>`編譯錯誤，因為老鼠不是繼承人類，不能放入建構式。
{% highlight kotlin linenos %}
class Obj<T : Human>(val obj: T) {
    fun print() {
        println(obj.toString())
    }
}

open class Human
data class Boy(val name: String, val age: Int) : Human()
data class Man(val name: String, val age: Int) : Human()
data class Mouse(val name: String, val age: Int, val weight: Int)

fun main() {
    val obj1 = Obj<Boy>(Boy("Bill", 5))
    val obj2 = Obj<Man>(Man("Jack", 30))
    val obj3 = Obj<Mouse>(Mouse("Kiki", 1, 1))
    obj1.print()
    obj2.print()
    obj3.print()
}
{% endhighlight %}

### 二個泛型類別類型
{% highlight kotlin linenos %}
class MyPair<K, V> (val first:K, val second: V) {
    override fun toString(): String {
        return "key = $first , value = $second"
    }
}
fun main() {
    val pair1 = MyPair<String, Int>("國語", 90)
    println(pair1.toString())
    val pair2 = MyPair<Int, Boolean>(1, true)
    println(pair2.toString())
}
{% endhighlight %}
```
key = 國語 , value = 90
key = 1 , value = true
```

## 泛型方法
使用方法:
```
fun <泛型類型> 方法名()
```
### 泛型類別與泛型方法
下面的程式碼中，<br>

`Box<T, U>`，此處的T是屬於泛型類別，`Box<String, Int>`，代表T為String。<br>

`fun <T> func1(param1:T)`此處的T是屬於泛型方法，`box.func1(false)`，代表T為Boolean。<br>

`fun func2(param1: T)`此處的T是屬於泛型類別，`Box<String, Int>`，代表T為String。<br>

雖然都是T，但不是相同的T，類型不相同。<br>
{% highlight kotlin linenos %}
class Box<T, U>(var item1: T, val item2: U) {
    fun <T> func1(param1: T) {
        println("param1 = $param1")
    }

    fun func2(param1: T) {
        item1 = param1
        println("item1 = $item1, item2 = $item2")
    }
}

fun main() {
    val box = Box<String, Int>("Hello", 10)
    box.func1(false)
    box.func2("World")
}
{% endhighlight %}
```
param1 = false
item1 = World, item2 = 10
```

### 泛型方法可以不用在類別中
關係運算子比較，T一定要是Comparable<T>的子類別。
{% highlight kotlin linenos %}
fun <T : Comparable<T>> max(a:T, b:T) : T {
    if (a > b) {
      return a
    } else {
      return b
    }
}

fun main() {
    println(max(10, 55))
    println(max(100.52, 100.55))
}
{% endhighlight %}
```
55
100.55
```

### 判斷泛型類型inline與reified
以下程式碼編譯錯誤
{% highlight kotlin linenos %}
fun <T> isInstanceOf(obj: Any): Boolean {
    return obj is T
}
{% endhighlight %}

判斷泛型類型一定要有`inline`與`reified`，缺一不可，使用內嵌函式可以知道類型。
{% highlight kotlin linenos %}
inline fun <reified T> isInstanceOf(obj: Any): Boolean {
    return obj is T
}
{% endhighlight %}

使用方法
```
isInstanceOf<判斷類型>(物件)
isInstanceOf<Man>(boy)
```

{% highlight kotlin linenos %}
fun main() {
    val boy = Boy("Bill", 20)
    val result = isInstanceOf<Man>(boy);
    println("result = $result")
}
{% endhighlight %}
```
result = false
```

### 轉型
轉型一定要有`inline`與`reified`，缺一不可，可能會轉失敗，所以回傳值是T?可空類型，因為是可空類型，使用as?可空類型。<br>
{% highlight kotlin linenos %}
inline fun <reified T> cast(obj: Any): T? {
    return obj as? T
}

open class Human
data class Boy(val name: String, val age: Int) : Human()

fun main() {
    val boy:Boy = Boy("Bill", 20)
    val person:Human? = cast<Human>(boy)
}
{% endhighlight %}

### 在類別中的泛型方法
以下程式碼，<R>是屬於泛型方法的類型，泛型類別沒有R這個類型。<br>

copyToMan()的func1參數為Lambda，回傳值是泛型方法的R類型。<br>

Lambda類型的參數是泛型類別的T類型，傳回值為泛型方法的R類型。<br>
{% highlight kotlin linenos %}
fun <R> copyToMan(func1:(T) -> R):R {
    return func1(obj)
}
{% endhighlight %}

Lambda傳入參數，把obj傳入func1這個Lambda
{% highlight kotlin linenos %}
class Obj<T>(val obj: T) {
    fun <R> copyToMan(func1:(T) -> R):R {
        return func1(obj)
    }
}
{% endhighlight %}

func1實際上是以下花括號{}的內容，it為上個程式碼obj的參數。<br>

建立Man()的物件，名字與Boy物件相同，年齡加上10。<br>

Lambda預設花括號{}最後一行就是回傳值，回傳值類型為R，R就是Man。<br>
{% highlight kotlin linenos %}
val obj1: Obj<Boy> = Obj(Boy("Bill", 5))
val man1 = obj1.copyToMan {
    Man(it.name, it.age.plus(10))
}
{% endhighlight %}

完整程式碼:
{% highlight kotlin linenos %}
class Obj<T>(val obj: T) {
    fun print() {
        println(obj.toString())
    }

    fun <R> copyToMan(func1:(T) -> R):R {
        return func1(obj)
    }
}

data class Boy(val name:String, val age:Int)
data class Man(val name: String, val age:Int)
data class Mouse(val name:String, val age:Int, val weight:Int)

fun main() {
    val obj1: Obj<Boy> = Obj(Boy("Bill", 5))
    val man1 = obj1.copyToMan {
        Man(it.name, it.age.plus(10))
    }
    println(man1.toString())
}
{% endhighlight %}
```
Man(name=Bill, age=15)
```

## 使用vararg產生泛型類別容器
以下是一個客制的List容器，建構子使用vararg，代表可以放入多個物件，物件的類型為T，任意類型。<br>

使用out是因為，out T是getItem()方法的傳回值類型，Kotlin規定方法的「傳入」參數為「泛型」為「in」，「傳回值」為傳出去的「泛型」類型為「out」。<br>

使用getItem(index)，可以透過索引取出，可能取不到，所以回傳類型為?可空類型. <br>
{% highlight kotlin linenos %}
class MyList<T>(vararg _items: T) {
    private var items:Array<out T> = _items
    fun getItem(index:Int):T? {
        return items[index]
    }
}
class Boy(val name:String, val age:Int)
class Man(val name: String, val age:Int)
fun main() {
    val items:MyList<Boy> = MyList(Boy("Bill", 5), Boy("Jack", 10), Boy("Kiki", 1))
    val obj1 = items.getItem(0)
    println("name = ${obj1?.name} age= ${obj1?.age}")
    val obj2 = items.getItem(1)
    println("name = ${obj2?.name} age= ${obj2?.age}")
}
{% endhighlight %}
```
name = Bill age= 5
name = Jack age= 10
```



[1]: {% link _pages/java/generics.md %}
[2]: {% link _pages/java/generics_method.md %}
[3]: {% link _pages/kotlin/variable.md %}
