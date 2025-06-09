---
title: 介面
date: 2025-04-18
keywords: Java, polymorphism, Interface
---
## 什麼是Interface?
介面(Interface)可以想成是「功能」，但功能的內容由實作(implements)的類別自己去完成。

實作介面的子類別，這個關係有點像是繼承，但不能把這個稱為繼承。

有一個Fly飛的功能，飛機去實作怎麼飛的功能，可以把飛機說它是一個Fly飛嗎？

只能說飛機可以像鳥一樣飛，Fly like a bird。

這是一個like a的關係，像...什麼什麼...一樣，有它的功能。

不能因為鳥有飛的功能，就把飛機繼承鳥。
{% highlight java linenos %}
class AirPlane extends Bird {
  
}
{% endhighlight %}

飛機跟鳥，不是Is A(是一個)，飛機不是一個鳥。

但飛機可以像鳥一樣飛，是Like A(像一個)的關係。

## 建立介面Interface
使用關鍵字interface來宣告這是一個介面(Interface)，這個介面(Interface)的名字是Fly。
{% highlight java linenos %}
public interface Fly {
}
{% endhighlight %}

## 屬性
介面的屬性存取權限一定是public，只能是final與static，final是無法再被實作的子類別修改，static是靜態屬性，static \+ final就是常數constant。

可以省略public final static，編譯會自動加上。
{% highlight java linenos %}
public interface Fly {
  public final static int VERSION = 1;
  // 上面這句與以下相等，二者是相同意思。
  // int VERSION = 1;
}
{% endhighlight %}

使用介面.屬性
{% highlight java linenos %}
System.out.println(Fly.VERSION);
{% endhighlight %}

無法再被更改。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(Fly.VERSION);
    // 以下會編譯失敗，因為是final，不可再被更改。
    Fly.VERSION = "addfdf";
  }
}
{% endhighlight %}

## 抽象方法
介面中的方法不寫內容，所以用分號`;`來結尾，由子類別去實作方法。

抽象方法存取權限一定是public，因為設為private就沒有辦法給子類別實作。

編譯器會自動加上為方法加上public、abstract。

{% highlight java linenos %}
public interface Fly {
  void fly();
  // 上面這句與以下相等，二者是相同意思。
  // public abstract void fly();
}
{% endhighlight %}

## 實作
所謂的實作就是把介面的抽象方法覆蓋掉，覆寫一個屬於自己的方法，即便內容是空的，只有花括號`{}`包住，也是覆寫。

使用`implements`關鍵字實作Fly介面，並且一定要覆寫(Override)抽象方法fly()
{% highlight java linenos %}
public class Airplane implements Fly{
  @Override
  public void fly() {
    // 即便內容是空的，只有花括號包住，也是覆寫。
  }
}
{% endhighlight %}

## 實作多個Interface
`implements Fly, Swim`  
以上語法使用逗號隔開不同介面，Fly, Swim都是介面(Interface)。

{% highlight java linenos %}
public class Duck implements Fly, Swim{
  // 覆寫Fly介面的fly()方法
  @Override
  public void fly() {}

  @Override
  public void swim() {}
}
{% endhighlight %}

## 介面的範例
飛的功能
{% highlight java linenos %}
public interface Fly {
  void fly();
}
{% endhighlight %}

游泳的功能
{% highlight java linenos %}
public interface Swim {
  void swim();
}
{% endhighlight %}

飛機實作飛的功能
{% highlight java linenos %}
public class AirPlane implements Fly {
  @Override
  public void fly() {
    System.out.println("飛機用引擎飛");
  }
}
{% endhighlight %}

鴨子實作飛與游泳的功能
{% highlight java linenos %}
public class Duck implements Fly, Swim{
  @Override
  public void fly() {
    System.out.println("鴨子用翅膀飛");
  }

  @Override
  public void swim() {
    System.out.println("鴨子用腳掌划");
  }
}
{% endhighlight %}

狗實作游泳的功能
{% highlight java linenos %}
public class Dog implements Swim {
  @Override
  public void swim() {
    System.out.println("狗游狗爬式");
  }
}
{% endhighlight %}

潛水艇實作游泳功能
{% highlight java linenos %}
public class Submarine implements Swim{
  @Override
  public void swim() {
    System.out.println("潛水艇會潛水");
  }
}
{% endhighlight %}

測試程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    AirPlane airPlane = new AirPlane();
    airPlane.fly();

    Duck duck = new Duck();
    duck.swim();
    duck.fly();

    Submarine submarine = new Submarine();
    submarine.swim();

    Dog dog = new Dog();
    dog.swim();
  }
}
{% endhighlight %}
```
飛機用引擎飛
鴨子會游泳
鴨子用翅膀飛
潛水艇會潛水
狗游狗爬式
```

## 多型
什麼是編譯類型？什麼是執行類型？

下方的程式碼，等號左邊是編譯類型，等號右邊是執行類型。

{% highlight java linenos %}
編譯類型     =  執行類型
Fly bird = new Bird();
Fly airplane = new Airplane();
{% endhighlight %}

介面作為類型，可以指向實作Fly介面的子類別。

雖然編譯類型是介面，但執行時指向的是實作介面的子類別。

呼叫介面的fly()，實際上是呼叫子類別的fly()。
{% highlight java linenos %}
Fly bird = new Bird();
bird.fly();
{% endhighlight %}

## 介面多型陣列
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 介面作為類型，只要是實作Fly的子類別都可以放進這個陣列
    Fly[] arr = new Fly[2];
    arr[0] = new AirPlane();
    arr[1] = new Bird();

    for (Fly obj : arr) {
      obj.fly();
    }
  }
}
class AirPlane implements Fly {
  @Override
  public void fly() {
    System.out.println("飛機飛");
  }
}
class Bird implements Fly {
  @Override
  public void fly() {
    System.out.println("鳥會飛");
  }
}
{% endhighlight %}
```
飛機飛
鳥會飛
```

## 判斷子類別
飛機有一個呼叫塔台的功能。
{% highlight java linenos %}
class AirPlane implements Fly {
  @Override
  public void fly() {
    System.out.println("飛機飛");
  }
  
  public void call() {
    System.out.println("呼叫塔台");
  }
}
{% endhighlight %}

使用instanceof與強制轉型成子類別，就可以呼叫子類別的方法。因為Fly介面只有fly()方法。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Fly[] arr = new Fly[2];
    arr[0] = new AirPlane();
    arr[1] = new Bird();

    for (Fly obj : arr) {
      if (obj instanceof AirPlane) {
        ((AirPlane) obj).call();
      }
      obj.fly();
    }
  }
}
{% endhighlight %}


## 介面的多型(參數)
寫一個fly()方法，參數是有實作Fly的類別都可以放進來。

寫一個Swim()方法，參數是有實作Swim的類別都可以放進來。
{% highlight java linenos %}
public class Test {
  public void fly(Fly something) {
    something.fly();
  }

  public void swim(Swim something) {
    something.swim();
  }
  
  public static void main(String[] args) {
    Test test = new Test();
    test.fly(new AirPlane());
    test.fly(new Duck());
    test.swim(new Duck());
    test.swim(new Submarine());
    test.swim(new Dog());
  }
}
{% endhighlight %}
```
飛機用引擎飛
鴨子用翅膀飛
鴨子會游泳
潛水艇會潛水
狗游狗爬式
```
由以上可以發現，介面跟繼承是完全不同的概念，類別(AirPlane)實作介面(Fly)的抽象方法(fly)，AirPlane就是介面(Fly)。

## 介面的多型
建立一個Operator運算子的介面，介面中有抽象的計算方法calculate()

Operator.java
{% highlight java linenos %}
// 運算子
public interface Operator {
  int calculate(int x, int y);
}
{% endhighlight %}

加法運算子，實作Operator介面，覆寫calculate()方法
{% highlight java linenos %}
public class Add implements Operator{
  @Override
  public int calculate(int x, int y) {
  return x + y;
  }
}
{% endhighlight %}

減法運算子，實作Operator介面，覆寫calculate()方法
{% highlight java linenos %}
public class Minus implements Operator{
  @Override
  public int calculate(int x, int y) {
  return x - y;
  }
}
{% endhighlight %}

乘法運算子，實作Operator介面，覆寫calculate()方法
{% highlight java linenos %}
public class Multiply implements Operator{
  @Override
  public int calculate(int x, int y) {
  return x * y;
  }
}
{% endhighlight %}

計算機，注意！calculate的第1個參數是Operator介面，也就是只要有實作Operator的類別(加法運算子、減法運算子、乘法運算子)都可以丟進去。
{% highlight java linenos %}
public class Calculator { 
  public int calculate(Operator operator, int x, int y) {
    return operator.calculate(x,y);
  }
}
{% endhighlight %}

主程式
{% highlight java linenos %}
  public static void main(String[] args) {
  // 建立計算機
  Calculator calculator = new Calculator();
  int x = 10;
  int y = 5;
  // 加法，第1個參數把實作Operator介面的Add類別丟進去
  int add_result = calculator.calculate(new Add(), x, y);
  System.out.println(x + "+" + y + " = " + add_result);

  // 減法，第1個參數把實作Operator介面的Minus類別丟進去
  int minus_result = calculator.calculate(new Minus(), x, y);
  System.out.println(x + "-" + y + " = " + minus_result);

  // 乘法，第1個參數把實作Operator介面的Multiply類別丟進去
  int multiply_result = calculator.calculate(new Multiply(), x, y);
  System.out.println(x + "*" + y + " = " + multiply_result);
  }
{% endhighlight %}
```
10+5 = 15
10-5 = 5
10*5 = 50
```

由上面例子發現，Operator介面可以是Add，可以是Minus、Multiply，類別只要實作Operator介面，這個類別就是Operator。

## Interface的default方法、static方法
jdk1.8之後有default方法、static方法。

前面有提過，介面有一點像繼承的關係，但又不是繼承，因為有一點像繼承，所以實作介面的子類別，都會自動擁有父介面的方法。

{% highlight java linenos %}
public interface Fly {
  int VERSION = 0;

  // 抽象方法
  void fly();

  // default方法
  default void dosomething() {
    fly();
  }

  // 靜態方法
  static void print() {
    System.out.println(VERSION);
  }
}
{% endhighlight %}

鳥實作Fly介面。
{% highlight java linenos %}
class Bird implements Fly {
  @Override
  public void fly() {
    System.out.println("鳥會飛");
  }
}
{% endhighlight %}

測試
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Bird bird = new Bird();
    // 鳥類擁有介面的屬性與方法
    bird.dosomething();
  }
}
{% endhighlight %}
```
鳥會飛
```

但物件沒有擁有介面的屬性，使用介面的屬性，仍是要用「類別.介面屬性」或「介面.屬性」。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Fly bird = new Bird();
    System.out.println(Bird.VERSION);
  }
}
{% endhighlight %}
