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

## 建構式與泛型類別
語法
```
class 類別名<T> {}
```
T是類型，不知道是什麼類型。<br>

泛型可以用在屬性、方法、也可以用在建構式。<br>

本範例是使用在主建構式，使用`_tmp`暫存變數，T類型可以是任意類型，把T類型的參數，指派給obj成員屬性。<br>

{% highlight kotlin linenos %}
class Obj<T>(_tmp: T) {
    private var obj:T = _tmp
}
{% endhighlight %}

以下有三個類別，分別是男孩、男人、老鼠。
{% highlight kotlin linenos %}
class Boy(val name:String, val age:Int)
class Man(val name: String, val age:Int)
class Mouse(val name:String, val weight:Int)
{% endhighlight %}

以下有三個變數obj1,obj2,obj3，使用Obj(物件)主要建構式，把物件設給obj成員屬性。<br>
{% highlight kotlin linenos %}
fun main() {
    val obj1: Obj<Boy> = Obj(Boy("Bill", 5))
    val obj2: Obj<Man> = Obj(Man("Jack", 30))
    val obj3: Obj<Mouse> = Obj(Mouse("Kiki", 1))
}
{% endhighlight %}

## 泛型方法
以下程式碼，<R>為新類型，泛型類別沒有R這個類型。<br>
transToMan()的參數為Lambda，回傳值是泛型方法的R類型。<br>
Lambda類型為參數是泛型類別的T類型，傳回值為泛型方法的R類型。<br>
{% highlight kotlin linenos %}
fun <R> transToMan(func1:(T) -> R):R {
    return func1(obj)
}
{% endhighlight %}

Lambda傳入參數，把obj傳入func1這個Lambda
{% highlight kotlin linenos %}
class Obj<T>(_tmp: T) {
    private var obj:T = _tmp
    fun <R> transToMan(func1:(T) -> R):R {
        return func1(obj)
    }
}
{% endhighlight %}

func1實際上是以下花括號{}的內容，it為上個程式碼obj的參數。<br>
建立Man()的物件，名字與Boy物件相同，年齡加上10。<br>
Lambda預設花括號{}最後一行就是回傳值，回傳值類型為R，R就是Man。<br>
{% highlight kotlin linenos %}
val man = obj1.transToMan {
    Man(it.name, it.age.plus(10))
}
{% endhighlight %}

完整程式碼:
{% highlight kotlin linenos %}
class Obj<T>(_tmp: T) {
    private var obj:T = _tmp
    fun <R> transToMan(func1:(T) -> R):R {
        return func1(obj)
    }
}
class Boy(val name:String, val age:Int)
class Man(val name: String, val age:Int)
fun main() {
    val obj1: Obj<Boy> = Obj(Boy("Bill", 5))
    val man = obj1.transToMan {
        Man(it.name, it.age.plus(10))
    }
    println("name = ${man.name} age= ${man.age}")
}
{% endhighlight %}
```
name = Bill age= 15
```

## 使用vararg產生泛型類別容器
以下是一個客制的List容器，建構子使用vararg，代表可以放入多個物件，物件的類型為T，任意類型。<br>
使用getItem(index)，可以透過索引取出，可能取不到，所以回傳類型為?可空類型. 
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
