---
title: 內部類別
date: 2025-04-18
keywords: Java, Inner class
---
## 內部類別Inner class
在一個類別中，宣告一個類別，這個類別就是內部類。

## 外部類別Outer Class
包含內部類別的類別稱為外部類別。
{% highlight java linenos %}
public class Outter { //外部類別
  public class Inner{ // 內部類別
    
  }
}
{% endhighlight %}

## 建立new內部類別
1. 先建立外部類別物件
2. 建立方式如下
```
外部類別物件變數.new 內部類別()
```

{% highlight java linenos %}
public class Outter { //外部類別
  public class Inner{ // 內部類別
    
  }
  public static void main(String[] args) {
    // 建立外部類別
    Outter outter = new Outter();
    // 再建立內部類別
    Inner inner = outter.new Inner();

  }  
}
{% endhighlight %}

## 內部類別建構子
{% highlight java linenos %}
public class Outter { //外部類別
  public class Inner{
    Inner() {
      System.out.println("內部類建構子");
    }
  }
  public static void main(String[] args) {
    // 建立內部類別
    Outter outter = new Outter();
    Inner inner = outter.new Inner();
  }
}
{% endhighlight %}
```
內部類建構子
```

## 存取外部類別的成員變數與成員方法
外部類別的成員變數、成員方法、靜態變數、靜態方法，在內部類別是可以存取。
{% highlight java linenos %}
public class Outter {
  private String name = "Outter";
  private static String staticval = "Static val";
  private static void staticMethod() {
    System.out.println("outterstatic method");
  }
  public void doSomething() {
    System.out.println("outter method");
  }
  public class Inner{
    void print() {
      doSomething(); // 呼叫外部方法
      System.out.println("outter name = " + name); // 使用外部變數
      staticMethod(); // 呼叫外部靜態方法
      System.out.println("outter staticval = " + staticval); //外部靜態變數
    }
  }
  public static void main(String[] args) {
    // 建立內部類別
    Outter outter = new Outter();
    Inner inner = outter.new Inner();
    inner.print();
  }
}
{% endhighlight %}
```
outter method
outter name = Outter
outterstatic method
outter staticval = Static val
```

## 內部類別是外部類別的成員
外部類別可以宣告一個內部類別的成員變數
{% highlight java linenos %}
public class Outter {
  private Inner inner; // 成員屬性是內部類別
  public class Inner{
    void print() {}
  }
}
{% endhighlight %}

## 外部類別可以用new建立內部類別
將成員變數指向內部類別「物件」。
{% highlight java linenos %}
public class Outter {
  private Inner inner; // 成員屬性是內部類別
  public class Inner{
    void print() {}
  }
  public Outter() {
    // 建立Outter物件的時候，建立內部類別
    inner = new Inner();
  }
}
{% endhighlight %}

## 外部類別可以使用內部類的方法
{% highlight java linenos %}
public class Outter {
  private Inner inner; // 成員屬性是內部類別
  public class Inner{
    void print() {}
  }
  public Outter() {
    // 建立Outter物件的時候，建立內部類別
    inner = new Inner();
    inner.print(); // 外部類別使用內部類別的方法
  }
}
{% endhighlight %}

## 內部類別可以有public、private、protected、default

## 私有內部類別
內部類別權限設為private，代表只有Outter這個外部類別可以存取，其它類別不能讀取到PrivateInner這個類別。

私有內部類別
{% highlight java linenos %}
public class Outter {
  private class privateInner {
    void print() {
      System.out.println("privateInner");
    }
  }
}
{% endhighlight %}

在其它類別呼叫
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();

    // 以下程式碼是無法執行的
    Outter.privateInner privateInner = outter.new privateInner();
    privateInner.print();
  }
}
{% endhighlight %}

## 區域內部類別
- 定義在方法或代碼塊中的類別
- 不能使用 public、private 或 protected 修飾
- 只能被該方法或程式碼區塊內的程式碼使用。

{% highlight java linenos %}
public class Test2 {
  public void method1() {
    class LocalInner {
      public void localMethod1() {
        System.out.println("區域內部類別中的方法");
      }
    }
    LocalInner localInner = new LocalInner();
    localInner.localMethod1();
  }

  public static void main(String[] args) {
    Test2 test = new Test2();
    test.method1();
  }
}
{% endhighlight %}
```
區域內部類別中的方法
```






