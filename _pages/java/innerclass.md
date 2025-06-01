---
title: 內部類別
date: 2025-04-18
keywords: Java, Inner class
---
## 內部類別與外部類別
在一個類別中，宣告一個類別，這個類別就是內部類別。

包含內部類別的類別稱為外部類別。

內部類別是外部類別的成員。

所謂的成員，就像是屬性、方法，都屬於類別的成員。

{% highlight java linenos %}
public class Outter { //外部類別
  public class Inner{ // 內部類別
    
  }
}
{% endhighlight %}

## 內部類別是一個類別
一般類別該有的存取權限(public、private、protected、default)、屬性、方法、建構子、匿名程式碼區塊、靜態程式碼區塊、靜態變數、靜態方法…等等，內部類別也有。

## 建立內部類別
### 使用new建立內部類別
1. 先建立外部類別物件
2. 建立方式如下
```
外部類別.內部類別 inner = 外部類別物件.new 內部類別()
```

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    Outter.Inner inner = outter.new Inner();;
  }
}
class Outter { //外部類別
  // 內部類別
  public class Inner { } 
}
{% endhighlight %}

### 外部類別提供public方法
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    Outter.Inner inner = outter.getInner();
  }
}
class Outter { //外部類別
  public Inner getInner() {
    return new Inner();
  }
  public class Inner {}
}
{% endhighlight %}

## 外部類別的成員屬性是內部類別
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    // 直接使用內部類別
    outter.inner.print();
  }
}
class Outter {
  public Inner inner; // 成員屬性是內部類別
  public class Inner{
    void print() {}
  }
  public Outter() {
    // 建立Outter物件的時候，建立內部類別
    inner = new Inner();
  }
}
{% endhighlight %}

## 外部類別可以存取private內部類別與屬性
內部類別權限設為private，代表只有外部類別可以存取，其它類別不能存取內部類別。

以下程式碼呼叫private方法與使用private內部類別
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    outter.callInner();
  }
}
class Outter { //外部類別
  public void callInner() {
    // 建立private內部類別物件
    Inner inner = new Inner();
    // 呼叫private方法
    inner.print();
  }
  private class Inner {
    private void print() {
      System.out.println("私有內部類別的方法");
    }
  }
}
{% endhighlight %}
```
私有內部類別的方法
```

## 內部類別建構子
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    Outter.Inner inner = outter.new Inner();;
  }
}
class Outter { //外部類別
  public class Inner{
    Inner() {
      System.out.println("內部類建構子");
    }
  }
}
{% endhighlight %}
```
內部類建構子
```

## 內部類別可以存取外部類別所有屬性
外部類別的private變數、方法、靜態變數、靜態方法，在內部類別是可以存取。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    Outter.Inner inner = outter.new Inner();
    inner.print();
  }
}
class Outter {
  private String name = "變數";
  private static String staticval = "靜態變數";
  private static void staticMethod() {
    System.out.println("靜態方法");
  }
  private void method1() {
    System.out.println("方法");
  }
  public class Inner{
    void print() {
      // 呼叫外部private方法
      method1();
      // 使用外部private變數
      System.out.println(name);
      // 呼叫外部private靜態方法
      staticMethod();
      // 外部private靜態變數
      System.out.println(staticval); 
    }
  }
}
{% endhighlight %}
```
方法
變數
靜態方法
靜態變數
```

## 內部類別繼承
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    outter.callInnerChild();
  }
}
class Outter {
  // 內部類別
  private class Inner{
    void print() {}
  }
  // 繼承內部類別
  private class InnerChild extends Inner {
    // 覆寫父類別的print()
    @Override
    void print() {
      System.out.println("test");
    }
  }
  public void callInnerChild() {
    // 建立private內部類
    InnerChild innerChild = new InnerChild();
    // 呼叫覆寫的方法
    innerChild.print();
  }
}
{% endhighlight %}
```
test
```

## 內部類別與外部類別屬性名字相同
內部類別有name，外部類別有name，使用Outter.this，就可以在內部類別取得外部類別的物件。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    Outter.Inner inner = outter.new Inner();
    inner.print();
  }
}
class Outter {
  // 外部類別name
  private String name = "Outter";
  public class Inner {
    // 內部類別name
    public String name = "Inner";
    public void print() {
      // 印出內部類別name
      System.out.println("Inner name = " + name);
      // 印出外部類別name
      System.out.println("Outter name = " + Outter.this.name);
    }
  }
}
{% endhighlight %}
```
Inner name = Inner
Outter name = Outter
```

## 外部類別.this
Outter.this與建立outter物件，hashCode都是一模一樣，為相同的物件。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter = new Outter();
    Outter.Inner inner = outter.new Inner();
    System.out.println(outter.hashCode());
    inner.print();
  }
}
class Outter {
  private String name = "Outter";
  public class Inner {
    public String name = "Inner";
    public void print() {
      System.out.println("Inner = " + Outter.this.hashCode());
    }
  }
}
{% endhighlight %}
```
989110044
Inner = 989110044
```

## 區域內部類別
- 定義在方法或匿名程式碼區塊中的類別
- 不能使用 public、private 或 protected 修飾
- 只能被該方法或匿名程式碼區塊的程式碼使用。

### 方法中的內部類別
{% highlight java linenos %}
public class Test2 {
  public void method1() {
    // 內部類別
    class LocalInner {
      public void method() {
        System.out.println("區域內部類別中的方法");
      }
    }
    LocalInner localInner = new LocalInner();
    localInner.method();
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
### 程式碼區塊的內部類別
將內部類別寫在`static{}`靜態程式碼區塊中。

不管建立幾次Test物件，在靜態程式碼區塊只執行一次，所以內部類別只被建立一次。
{% highlight java linenos %}
public class Test {
  static {
    class Inner {
      public void method() {
        System.out.println("呼叫內部類別方法");
      }
    }
    Inner inner = new Inner();
    inner.method();
  }
  public static void main(String[] args) {
    // 建立3次物件
    Test test1 = new Test();
    Test test2 = new Test();
    Test test3 = new Test();
  }
}
{% endhighlight %}
```
呼叫內部類別方法
```

### 區域內部類別繼承
區域內部類別可以被繼承。
{% highlight java linenos %}
public class Test {
  public void method1() {
    class LocalInner {
      public void method() {
        System.out.println("區域內部類別中的方法");
      }
    }
    class InnerChild extends LocalInner{
      @Override
      public void method() {
        System.out.println("子類別覆寫");
      }
    }
    LocalInner child = new InnerChild();
    child.method();
  }
  public static void main(String[] args) {
    Test test = new Test();
    test.method1();
  }
}
{% endhighlight %}
```
子類別覆寫
```




