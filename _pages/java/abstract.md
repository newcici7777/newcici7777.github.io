---
title: 抽象
date: 2025-04-18
keywords: Java, polymorphism, Upcasting object, Downcasting object, Abstract, Abstract class, Abstract method
---
Prerequisites:
- [多型][1]

## 抽象類別
在class前面加上abstract
用關鍵字abstract修飾的類別稱為抽象類別(abstract class)
{% highlight java linenos %}
abstract class Parent {	
}
{% endhighlight %}

## 抽象方法
在方法傳回值類型前面加上abstract，注意!最後面不能有花括號`{}`方法主體，直接以分號`;`結束。

用關鍵字abstract修飾的方法稱為抽象方法(abstract method)
{% highlight java linenos %}
abstract int speak();
{% endhighlight %}

## 繼承
所謂的繼承是繼承相同的行為，比如人，人會說話，而說話就是行為，在程式語言中，我們把人視作一個類別，而說話的行為，在Java中視為類別中的成員方法，在C++中視為類別的成員函式，成員函式與成員方法都是相同的東西。

繼承相同的屬性，比如人，人有眼睛，而眼睛就是屬性，在程式語言中，我們把人視作一個類別，而眼睛是屬性，在Java中視為類別中的成員屬性，在C++中視為類別的成員變數，成員屬性與成員變數都是相同的東西。

人是一種概念，而這種概念就是父類別，具體是什麼人？是黃種人？是白人？是黑人？黃種人、白人、黑人就是子類別。

當然子類別也可以是一種概念，因為黃種人下面還有中國人、日本人、韓國人、菲律賓人、越南人、泰國人、馬來西亞人，這些細分下來各個國家的人就是非常具體的子類別，所謂的非常具體(特殊化Specialization)，就是類別無法再作為概念再細分下去，這種就是確確實實的子類別。

## 為什麼要使用抽象類別？
人是一種概念，最原始的人會說什麼語言，不是重點，大家關心的是什麼國家的人會說什麼語言，比如中國人說中文、日本人說日文、韓國人說韓文、越南人說越語、泰國人說泰語、美國人說英文、西班牙人說西班牙語、俄羅斯人說俄文，有人會在意概念中的「人」會說「什麼話」嗎？既然沒有人會關心概念中的「人」會說「什麼話」，那乾脆就不做事，不用實作speak()這個行為。

人是一種概念，「人」會說「什麼話」，這個「什麼話」，也是一種概念，既然是都是概念，真正是「什麼人」與「說什麼話」，就由子類別去覆寫「說什麼話」，子類別去成為「什麼人」。

## 抽象類別不能new，不能建立
因此「人」這個概念，與「說什麼話」這個概念，就不描述裡面如何運作，也不能建立人這個「概念」。
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
先前提到人下面有亞洲人、歐洲人、非洲人這些概念，概念的繼承，不用把「說話」這個行為實作出來，由具體的中國人、日本人、韓國人...等等，去實作說話這個行為就好。

以下是「亞洲人」繼承「人」，不用覆寫怎麼說話speak()的方法。
{% highlight java linenos %}
abstract class Asian extends Human{
}
{% endhighlight %}

## 子類別繼承抽象類別
中國人是一個非常具體的子類別，子類別繼承抽象類別，就一定要覆寫抽象方法。
{% highlight java linenos %}
public class Chinese extends Asian{
  @Override
  void speak() {
      System.out.println("說中文");
  }
}
{% endhighlight %}

## 抽象類別中可以有「非」抽象方法
以下是人的類別，但是有睡覺這個非抽象方法，所有身為人一定會有的共同行為，不管是亞洲人、歐洲人、非洲人都一定會有睡覺這個「行為」，因為沒辦法客製化不同的人種是怎麼睡覺，所以不使用abstract，不強制一定要重寫sleep()這個方法，但sleep()這個行為，只要是繼承Human，都一定擁有這個sleep()的方法。
{% highlight java linenos %}
abstract class Human {
  abstract void speak();
  void sleep() {
    System.out.println("作夢");
  }
}
{% endhighlight %}

## 抽象類別向上轉型物件
在多型的文章中有提到，向上轉型物件(Upcasting object)，使用子類別的建立物件的建構子，「產生出父類別的物件」，稱之為向上轉型物件Upcasting object。

抽象類別向上轉型物件也是同樣的意思，使用子類別的建立物件的建構子，「產生出抽象父類別的物件」。
{% highlight java linenos %}
public class Teest {
  public static void main(String[] args) {
    Human upcasting_obj1 = new Chinese();
    upcasting_obj1.speak();
  }
}
{% endhighlight %}

注意！先前提到抽象類別不能new，但可以作為向上轉型物件，因為實際是用「子類別的建構子」，產生出「抽象類別的物件」，向上轉型物件並不是抽象類別，也不是子類別，這個概念在多型的文章已提到過了。

## 抽象類多型
所謂的多型就是，抽象類別物件指向多種不同子類別，抽象類別可以使用「子類別覆寫抽象類別的方法」。  
範例如下
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

根據物件導向程式設計(Object-oriented programming)原則，設計重要的物件的時候要讓父類別或抽象類別指向子類別物件，而不是讓具體的子類別指向具體子類別物件。

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

若讓子類別型別指向具體子類別物件，這個播放器就要為setSpeaker()方法設置各國人的參數，使用向上轉型物件，就可以解決以下要寫好多個參數問題(具體的子類別型別指向子類別物件)，向上轉型物件可以使用不同子類別「覆寫父類別的方法」。
{% highlight java linenos %}
public void setSpeaker(Chinese speaker) {}
public void setSpeaker(American speaker) {}
public void setSpeaker(Japanese speaker) {}
public void setSpeaker(Korean speaker) {}
public void setSpeaker(Spanish speaker) {}
{% endhighlight %}

[1]: {% link _pages/java/polymorphism.md %}
