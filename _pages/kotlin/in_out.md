---
title: in out 泛型轉型
date: 2025-09-12
keywords: kotlin, in out
---
泛型轉型主要是透過「泛型介面」轉型。

## Interface 介面
### Product生產者介面
out視為輸出，方法只能傳回return T類型的物件。<br>以下是生產者介面，只有一個方法，product()負責生產T類型的東西。<br>
實作的子類別，必須實作product()方法。<br>
{% highlight kotlin linenos %}
interface Production<out T> {
    fun product() : T
}
{% endhighlight %}

### Consumer消費者介面
in視為輸入，方法只能傳入T類型的參數，不能有傳回值。<br>以下是消費者介面，只有一個方法，consume()負責接收T類型參數。<br>
實作的子類別，必須實作consume()方法。<br>
{% highlight kotlin linenos %}
interface Consumer<in T> {
    fun consume(item:T)
}
{% endhighlight %}

## 父類別與子類別
父類別是Animal，子類別是Fish
{% highlight kotlin linenos %}
open class Animal

class Fish:Animal()
{% endhighlight %}

## 實作Interface
### 覆寫Product介面方法
實作的Product介面的子類別，一定要覆寫product()方法。
{% highlight kotlin linenos %}
// 賣魚商店
class FishStroe:Production<Fish> {
    override fun product(): Fish {
        return Fish()
    }
}

// 賣動物商店
class AnimalStroe:Production<Animal> {
    override fun product(): Animal {
        return Animal()
    }
}
{% endhighlight %}

### 覆寫Consumer介面方法
實作的Consumer介面的子類別，一定要覆寫consume()方法。
{% highlight kotlin linenos %}
// 買魚的消費者
class FishConsumer:Consumer<Fish> {
    override fun consume(item: Fish) {
        println("eat fish")
    }
}

// 買動物的消費者
class AnimalConsumer:Consumer<Animal> {
    override fun consume(item: Animal) {
        println("eat Animal")
    }
}
{% endhighlight %}

## 泛型類別透過介面轉型
### 子類泛型轉父類泛型
下面程式碼，等號左邊是Production介面，泛型類型是Animal，但實際上是FishStore的類別，執行product()方法，收到的是魚Fish物件。<br>
{% highlight kotlin linenos %}
    val animalStroe:Production<Animal> = FishStroe()
    val obj = animalStroe.product()
    println(obj.toString())
{% endhighlight %}
```
Fish@1be6f5c3
```
### 父類泛型轉子類泛型
下面程式碼，等號左邊是Consumer介面，泛型類型是Fish，等號右邊指派的是AnimalConsumer，呼叫consume()方法，實際上是執行AnimalConsumer.consume()方法。<br>
{% highlight kotlin linenos %}
    val fishConsumer:Consumer<Fish> = AnimalConsumer()
    fishConsumer.consume(Fish())
{% endhighlight %}
```
eat Animal
```

## Java無法泛型轉型
### 子類泛型轉父類泛型
以下編譯不過。
{% highlight java linenos %}
List<Animal> list = new ArrayList<Fish>();
{% endhighlight %}

Kotlin透過泛型介面，可以子類泛型轉型成父類泛型。
{% highlight kotlin linenos %}
val animalStroe:Production<Animal> = FishStroe()
{% endhighlight %}

### 父類泛型轉子類泛型
以下編譯不過。
{% highlight java linenos %}
List<Fish> list = new ArrayList<Animal>();
{% endhighlight %}

Kotlin透過泛型介面，可以父類泛型轉型成子類泛型。<br>
{% highlight kotlin linenos %}
val fishConsumer:Consumer<Fish> = AnimalConsumer()
{% endhighlight %}
