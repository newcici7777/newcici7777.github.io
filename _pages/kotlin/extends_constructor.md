---
title: extends constructor
date: 2025-06-02
keywords: kotlin, extends constructor
---
Prerequisites:

- [constructor][1]

## open
Kotlin預設是不能被繼承，若要讓類別可以被繼承，在class前面加上open。

{% highlight kotlin linenos %}
open class Parent {  // 使用open class
}
{% endhighlight %}

## 父類別無主要建構式
### 父類別預設主要建構式
父類別沒寫主要建構子，會有一個預設的主要建構式產生。
{% highlight kotlin linenos %}
open class Parent {
}
{% endhighlight %}

預設的主要建構式。
{% highlight kotlin linenos %}
open class Parent constructor() {
}
{% endhighlight %}

### 子類別繼承父類別
子類別繼承父類別時，要呼叫主要建構式，即便父類別沒寫主要建構式，子類別還是要用父類別\(\)，呼叫主要建構式。
{% highlight kotlin linenos %}
// 父類別沒寫主要建構子
open class Parent {

}
// 子類別繼承時要呼叫父類別的主要建構式()
class Child : Parent() {

}
{% endhighlight %}

## 父類別有主要建構式
{% highlight kotlin linenos %}
open class Parent(val name: String, val age: Int) {
}
{% endhighlight %}

### 子類別繼承父類別
1. 子類別要把父類別的建構式的參數，寫在子類別的主要建構式中。
2. 子類別主要建構式的參數，不能有val、var。
3. 把子類別主要建構式的參數，代入繼承父類別建構式中。
4. 參數會代入父類別的主要建構式。

![img]({{site.imgurl}}/kotlin/extends_c1.png)

![img]({{site.imgurl}}/kotlin/extends_c2.png)

![img]({{site.imgurl}}/kotlin/extends_c3.png)

{% highlight kotlin linenos %}
class Child(name: String, age:Int) : Parent(name, age) {
}
{% endhighlight %}

為什麼要這樣做？因為繼承的時候，要呼叫父類別的主要建構式，建立父類別屬性。

就像Java中，子類別繼承父類別，預設會呼叫super()，建立父類別。

## 子類別沒有主要建構式
若子類別沒有主要建構式，要在次要建構式中，使用super，呼叫父類別的主要建構式。

步驟如下:
1. 繼承，不用 : 父類別(參數, 參數)，直接 : 父類別
2. 使用constructor關鍵字，建立次要建構式。
3. 把父類別的主要建構式的參數，寫在子類別的次要建構式中。
4. 子類別次要建構式的參數，不能有val、var。
5. 使用super關鍵字，把子類別次要建構式的參數，代入繼承父類別建構式。
6. 參數會代入父類別的主要建構式。

![img]({{site.imgurl}}/kotlin/extends_sc1.png)

{% highlight kotlin linenos %}
class Child: Parent {
    constructor(name: String, age:Int) : super(name, age) {
    }
}
{% endhighlight %}

## 繼承父類次要建構式
### 父類別有主要建構式和次要建構式
{% highlight kotlin linenos %}
open class Parent(val name: String, val age: Int) {
    var hobby: String = ""

    constructor(name: String, age: Int, hobby: String) :
            this(name, age) {
        this.hobby = hobby
    }
}
{% endhighlight %}

子類別繼承呼叫父類次要建構式。
{% highlight kotlin linenos %}
class Child : Parent {
    constructor(name: String, age: Int, hobby: String) :
            super(name, age, hobby) {
    }
}
{% endhighlight %}

### 父類別沒有主要建構式
{% highlight kotlin linenos %}
open class Parent {
    constructor(name: String) {
    }
}
{% endhighlight %}

子類別繼承呼叫父類次要建構式。
{% highlight kotlin linenos %}
class Child : Parent {
    constructor(name: String) : super(name) {
    }
}
{% endhighlight %}

## 主要建構式權限修飾子
若主要建構式有權限修飾子，要把`constructor`寫出來，次建構式要用this()呼叫主要建構式。
{% highlight kotlin linenos %}
open class Parent protected constructor() {
    constructor(name: String) : this() {
    }
}
{% endhighlight %}

## 結論
不管如何，都要繼承其中一個父類建構式，管它是主要建構式或次要建構式，一定要繼承其中一個。

[1]: {% link _pages/kotlin/constructor.md %}