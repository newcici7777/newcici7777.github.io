---
title: 類別
date: 2025-06-02
keywords: kotlin, class
---
## 類別body
body又稱為主體，包含屬性、方法、建構子、內部類別。
```
class 類別名 {
  // 類別body
}
```

Student類別。
{% highlight kotlin linenos %}
class Student {
    // 屬性
    var name = "Alice"
}
{% endhighlight %}

## 建立物件
建立物件，並把物件指派給變數。
```
var 變數 = 類別名()
val 變數 = 類別名()
```

建立物件，並把物件指派給student變數。
{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
fun main() {
    // 建立物件
    var student = Student()
}
{% endhighlight %}

## val物件變數
此處的val並不是不能修改物件的屬性，而是不能再把val物件變數指向其它物件。

{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
fun main() {
    // val物件變數
    val student2 = Student()
    // 以下會出錯 因為Val cannot be reassigned
    student2 = Student()
}
{% endhighlight %}

val物件變數，可以修改物件的屬性，以下程式碼把name指派新的值。
{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
fun main() {
    val student2 = Student()
    student2.name = "Doris"
    println(student2.name)
}
{% endhighlight %}
```
Doris
```

## 方法
方法為類別的行為、動作。

在類別中，宣告一個方法用fun開頭。

{% highlight kotlin linenos %}
class Student {
    fun goToSchool() {

    }
    fun takeClass() {

    }
}
{% endhighlight %}

方法互相呼叫。
{% highlight kotlin linenos %}
class Student {
    fun goToSchool() {
        takeClass()
    }
    fun takeClass() {

    }
}
{% endhighlight %}

物件呼叫方法。
{% highlight kotlin linenos %}
    val student = Student()
    student.takeClass()
{% endhighlight %}

