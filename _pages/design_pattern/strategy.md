---
title: 策略模式
date: 2025-04-30
keywords: Java, Design patterns, Strategy Pattern
---
建一個鳥的類別，然後要定義飛這個行為。
{% highlight java linenos %}
class Bird {
  void fly();
}
{% endhighlight %}

但所有的鳥類都會飛嗎？公雞就不會飛、駝鳥也不會飛，鴨子會飛一些，所以要把會飛這個行為變成一種策略，有些鳥類飛的很好，有些會飛一些，有些不會飛。

1. 建立FlyBehavior介面。
2. 實作flyBehavior，建立「會飛」、「飛很差」、「不會飛」三種飛的行為。
3. 鳥的類別把flyBehavior作為成員變數。
4. 鳥的fly()方法呼叫flyBehavior.fly()，如果flyBehavior有設定的話。
3. 鳥的「子」類別去設定飛的行為。

![img]({{site.imgurl}}/pattern/strategy1.png)

FlyBehavior飛的行為，有fly()方法

FlyWell、NotFly、FlyBadly實作FlyBehavior介面

Bird為抽象類別有flyBehavior成員變數與fly()方法

Chicken、Sparrow麻雀、Douck為Bird的子類。

## FlyBehavior介面
{% highlight java linenos %}
public interface FlyBehavior {
  // 抽象方法
  void fly();
}
{% endhighlight %}

## FlyWell,NotFly,FlyBadly
實作FlyBehavior，都要覆寫抽象方法fly()
{% highlight java linenos %}
public class FlyWell implements FlyBehavior {
  @Override
  public void fly() {
    System.out.println("Fly very well");
  }
}

public class NotFly implements FlyBehavior {
  @Override
  public void fly() {
    System.out.println("Can not fly.");
  }
}

public class FlyBadly implements FlyBehavior {
  @Override
  public void fly() {
    System.out.println("Fly very Badly.");
  }
}
{% endhighlight %}

## Bird抽象類別
flyBehavior設為成員屬性
{% highlight java linenos %}
public abstract class Bird {
  // 成員屬性
  protected FlyBehavior flyBehavior;
  
  // 聲音
  public abstract void sound();
  
  // 飛的方法
  public void fly() {
    // 如果有飛的行為，就用
    if (flyBehavior != null)
      flyBehavior.fly();
  }
}
{% endhighlight %}

## Chicken,Sparrow,Duck
子類別使用建構子設定飛的行為
{% highlight java linenos %}
public class Chicken extends Bird {
  public Chicken() {
    // 建構子設定飛的行為
    flyBehavior = new NotFly();
  }

  @Override
  public void sound() {
    System.out.println("咕咕咕");
  }
}

public class Sparrow extends Bird{
  public Sparrow() {
    // 建構子設定飛的行為
    flyBehavior = new FlyWell();
  }

  @Override
  public void sound() {
    System.out.println("吱吱");
  }
}

public class Duck extends Bird {
  public Duck() {
    // 建構子設定飛的行為
    flyBehavior = new FlyBadly();
  }

  @Override
  public void sound() {
    System.out.println("嘎嘎");
  }
}
{% endhighlight %}

## main主程式
可以動態修改飛的行為
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Chicken chicken = new Chicken();
    System.out.println("=== chicken ===");
    chicken.fly();
    Sparrow sparrow = new Sparrow();
    System.out.println("=== Sparrow ===");
    sparrow.fly();
    Duck duck = new Duck();
    System.out.println("=== duck ===");
    duck.fly();
    System.out.println("=== setting duck again ===");
    // 動態修改飛的行為
    duck.flyBehavior = new FlyWell();
    duck.fly();
  }
}
{% endhighlight %}
```
=== chicken ===
Can not fly.
=== Sparrow ===
Fly very well
=== duck ===
Fly very Badly.
=== setting duck again ===
Fly very well
```