---
title: 抽象
date: 2025-04-18
keywords: Java, polymorphism, Abstract class, Abstract method
---
Prerequisites:
- [多型][1]

## 抽象類別
用關鍵字abstract修飾的類別稱為抽象類別(abstract class)
{% highlight java linenos %}
abstract class Parent {	
}
{% endhighlight %}

## 抽象方法
在方法傳回值類型前面加上abstract，注意!最後面不能有花括號`{}`方法主體，直接以分號`;`結束。

{% highlight java linenos %}
abstract int speak();
{% endhighlight %}

## 為什麼要使用抽象類別？
不能使用new來建立類別，就是抽象類別。

## 抽象類別不能new，不能建立
以下是人的抽象類別。
{% highlight java linenos %}
abstract class Human {
  abstract void speak();
}
{% endhighlight %}

以下語法會編譯錯誤，抽象類別不能new，不能建立。
{% highlight java linenos %}
Human human = new Human();
{% endhighlight %}

## 抽象類別繼承抽象類別
以下是「亞洲人」繼承「人」，不用覆寫speak()的方法。
{% highlight java linenos %}
abstract class Asian extends Human{
}
{% endhighlight %}

## 子類別繼承抽象類別
中國人繼承抽象類別，就一定要覆寫抽象方法。
{% highlight java linenos %}
public class Chinese extends Asian{
  @Override
  void speak() {
      System.out.println("說中文");
  }
}
{% endhighlight %}

## 抽象類別中可以有「非」抽象方法
以下是人的類別，但是有睡覺這個非抽象方法，所有身為人一定會有的共同行為，只要是繼承Human，都一定擁有這個sleep()的方法。
{% highlight java linenos %}
abstract class Human {
  abstract void speak();
  // 非抽象方法
  void sleep() {
    System.out.println("作夢");
  }
}
{% endhighlight %}

## 抽象類別多型
多型，類型是父類別，但實際上指向的是子類別物件。

以下類型是Human,實際上是Chinese。
{% highlight java linenos %}
public class Teest {
  public static void main(String[] args) {
    Human obj1 = new Chinese();
    obj1.speak();
  }
}
{% endhighlight %}

## 抽象類別多型範例
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 抽象類別的物件指向美國人
    Human human = new American();
    // 使用子類別覆寫抽象類別的方法
    human.speak();

    // 抽象類別的物件指向中國人
    human = new Chinese();
    // 使用子類別覆寫抽象類別的方法
    human.speak();

    // 抽象類別的物件指向日本人
    human = new Japanese();
    // 使用子類別覆寫抽象類別的方法
    human.speak();
  }
}
{% endhighlight %}

## OOP原則
為什麼要使用父類別物件指向子類別，而不是使用子類別指向子類別物件？

根據設計模式原則，寫程式是針對抽象、介面而寫，而不是針對類別而寫。

以下建立一個聲音播放器物件，可以接收不同國家的人的說話聲音。
{% highlight java linenos %}
public class RedioPlayer {
  private Human speaker;
  public void setSpeaker(Human speaker) {
    this.speaker = speaker;
  }
  public void play() {
    speaker.speak();
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    RedioPlayer player = new RedioPlayer();
    // 說中文的播放器
    player.setSpeaker(new Chinese());
    player.play();
    
    // 說英文的播放器
    player.setSpeaker(new American());
    player.play();

    // 說日文的播放器
    player.setSpeaker(new Japanese());
    player.play();
  }
}
{% endhighlight %}
```
說中文
speak english.
說日文
```

若讓型別指向底層類別，這個播放器就要為setSpeaker()方法設置各國人的參數。
{% highlight java linenos %}
public void setSpeaker(Chinese speaker) {}
public void setSpeaker(American speaker) {}
public void setSpeaker(Japanese speaker) {}
public void setSpeaker(Korean speaker) {}
public void setSpeaker(Spanish speaker) {}
{% endhighlight %}

[1]: {% link _pages/java/polymorphism.md %}
