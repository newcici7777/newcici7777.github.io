---
title: 介面Interface
date: 2025-04-18
keywords: Java, polymorphism, Interface
---
## 什麼是Interface?
介面(Interface)可以想成是「功能」，但功能的內容由實作(implements)的類別自己去完成。
介面跟繼承不相關，只在乎有沒有實作(implements)介面(Interface)。

## 建立介面Interface
使用關鍵字interface來宣告這是一個介面(Interface)，這個介面(Interface)的名字是Callback。
{% highlight java linenos %}
public interface Callback {
}
{% endhighlight %}

## Interface只有常數與抽象方法
介面(Interface)只有常數，沒有變數。  
介面(Interface)只有抽象方法(Abstract method)，不是抽象方法不能放在介面(Interface)中。

## 常數
常數就是永恒不變的值，介面(Interface)的常數存取權限一定是public，只能是final。

可以省略public final，編譯會自動加上。
{% highlight java linenos %}
public interface Callback {
  public final int ERR_CODE = 404;
  // 上面這句與以下相等，二者是相同意思。
  // int ERR_CODE = 404;
}
{% endhighlight %}

## 抽象方法
抽象「方法」存取權限一定是public。  
public、abstract可以省略，編譯器會自動加上。  
{% highlight java linenos %}
public interface Callback {
  void sendMessage(int msg);
  // 上面這句與以下相等，二者是相同意思。
  // public abstract void sendMessage(int msg);
}
{% endhighlight %}

## 實作Interface
實作(implements)介面(Interface)的類別就一定要覆寫(Override)介面(Interface)的抽象方法(Abstract method)，否則編譯器會產生錯誤。

使用`implements`關鍵字實作Callback介面，並且一定要覆寫(Override)抽象方法sendMessage()
{% highlight java linenos %}
public class Player implements Callback{
  @Override
  public void sendMessage(int msg) {}
}
{% endhighlight %}

## 實作多個Interface
`implements Callback, Runnable`  
以上語法使用逗號隔開不同介面，Callback, Runnable都是介面(Interface)。

{% highlight java linenos %}
public class Player implements Callback, Runnable{
  // 覆寫Callback介面的sendMessage()方法
  @Override
  public void sendMessage(int msg) {}

  // 覆寫Runnable介面的run()方法
  @Override
  public void run() {}
}
{% endhighlight %}

## 介面的範例
建立一個Operator運算元的介面(Interface)，介面中有計算的方法calculate()，由實作此介面的類別去完成。

Operator.java
{% highlight java linenos %}
// 運算元
public interface Operator {
  /**
   * 計算功能
   */
  int calculate(int x, int y);
}
{% endhighlight %}

加法運算元，實作Operator介面，覆寫(Override)calculate()方法
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

減法運算元，實作Operator介面，覆寫(Override)calculate()方法
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

乘法運算元，實作Operator介面，覆寫(Override)calculate()方法
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

計算機，注意！calculate的第1個參數是Operator介面，也就是只要有實作Operator的類別(加法運算元、減法運算元、乘法運算元)都可以丟進去。
{% highlight java linenos %}
public class Calculator {
  /**
   * 計算
   * @param operator 運算元
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

## 介面的多型
由上面例子發現，Operator介面可以是Add，可以是Minus、Multiply，一個介面有多種類型，類別只要實作Operator介面，我們就會說這個類別就是Operator介面。



