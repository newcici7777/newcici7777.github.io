---
title: 介面
date: 2025-04-18
keywords: Java, polymorphism, Interface
---
## 什麼是Interface?
介面(Interface)可以想成是「功能」，但功能的內容由實作(implements)的類別自己去完成。
介面跟繼承不相關，只在乎有沒有實作(implements)介面(Interface)。

## 建立介面Interface
使用關鍵字interface來宣告這是一個介面(Interface)，這個介面(Interface)的名字是Fly。
{% highlight java linenos %}
public interface Fly {
}
{% endhighlight %}

## Interface只有常數與抽象方法
介面(Interface)只有常數，沒有變數。  
介面(Interface)只有抽象方法(Abstract method)，不是抽象方法不能放在介面(Interface)中。

## 常數
常數就是永恒不變的值，介面(Interface)的常數存取權限一定是public，只能是final。

可以省略public final，編譯會自動加上。
{% highlight java linenos %}
public interface Fly {
  public final int VERSION = 1;
  // 上面這句與以下相等，二者是相同意思。
  // int VERSION = 1;
}
{% endhighlight %}

## 抽象方法
抽象「方法」存取權限一定是public。  
public、abstract可以省略，編譯器會自動加上。  
{% highlight java linenos %}
public interface Fly {
  void fly();
  // 上面這句與以下相等，二者是相同意思。
  // public abstract void fly();
}
{% endhighlight %}

## 實作Interface
實作(implements)介面(Interface)的類別就一定要覆寫(Override)介面(Interface)的抽象方法(Abstract method)，否則編譯器會產生錯誤。

使用`implements`關鍵字實作Fly介面，並且一定要覆寫(Override)抽象方法fly()
{% highlight java linenos %}
public class Airplane implements Fly{
  @Override
  public void fly() {}
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
  /**
   * 計算功能
   */
  int calculate(int x, int y);
}
{% endhighlight %}

加法運算子，實作Operator介面，覆寫(Override)calculate()方法
{% highlight java linenos %}
public class Add implements Operator{
  /***
   * 實作加法計算
   */
  @Override
  public int calculate(int x, int y) {
  return x + y;
  }
}
{% endhighlight %}

減法運算子，實作Operator介面，覆寫(Override)calculate()方法
{% highlight java linenos %}
public class Minus implements Operator{
  /***
   * 實作減法計算
   */
  @Override
  public int calculate(int x, int y) {
  return x - y;
  }
}
{% endhighlight %}

乘法運算子，實作Operator介面，覆寫(Override)calculate()方法
{% highlight java linenos %}
public class Multiply implements Operator{
  /***
   * 實作乘法計算
   */
  @Override
  public int calculate(int x, int y) {
  return x * y;
  }
}
{% endhighlight %}

計算機，注意！calculate的第1個參數是Operator介面，也就是只要有實作Operator的類別(加法運算子、減法運算子、乘法運算子)都可以丟進去。
{% highlight java linenos %}
public class Calculator {
  /**
   * 計算
   * @param operator 運算子
   * @param x
   * @param y
   * @return
   */
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



