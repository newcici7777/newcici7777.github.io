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

## 靜態內部類別
進行以下的內容，請先讀過以下2篇文章:
- [Static][1]
- [建構子][2]

### 使用靜態內部類別
什麼是靜態內部類別？其實也是一個類別，只是外部類別變成名稱空間，呼叫靜態內部類別的靜態方法，這樣呼叫:
{% highlight java linenos %}
外部類別.靜態內部類別.靜態方法();
{% endhighlight %}

呼叫靜態方法時，類別名也變成名稱空間，這樣呼叫:
{% highlight java linenos %}
類別.靜態方法();
{% endhighlight %}

以上呼叫的方法如出一徹。

### static關鍵字
在內部類前面加上static的關鍵字就變成靜態內部類別。
{% highlight java linenos %}
public class Outter {
  public static class InnerStaticClass {
    
  }
}
{% endhighlight %}

### 建立new靜態內部類別
語法
```
外部類別.內部類別 變數 = new 外部類別.內部類別()
```

注意！靜態內部類別建立的方式為「內部類別」差距很大，不相同。

{% highlight java linenos %}
public class Test {
  public static class Static_class {
  }

  public static void main(String[] args) {
    Test.StaticClass staticClass = new Test.StaticClass();
    staticClass.inner_memthod();
  }
}
{% endhighlight %}

如果在外部類別中的main靜態函式建立類別，前面可以不用外部類別。

{% highlight java linenos %}
public class Test {
  public static class Static_class {
  }

  public static void main(String[] args) {
    StaticClass staticClass = new StaticClass();
    staticClass.inner_memthod();
  }
}
{% endhighlight %}

### 靜態內部類別建構子
之前提過靜態內部類別就是類別，只是外部類別變成名稱空間，所以一定有建構子。
{% highlight java linenos %}
public class Test {
  public static class Static_class {
    Static_class() {
      System.out.println("靜態內部類別建構子");
    }
  }
  public static void main(String[] args) {
    Test.Static_class staticClass = new Test.Static_class();
  }
}
{% endhighlight %}
```
靜態內部類別建構子
```

### 靜態內部類別的靜態方法
- 只能存取外部靜態變數與靜態方法
- 只能存取內部靜態變數與靜態方法

{% highlight java linenos %}
public class Test {
  public static String static_name = "外部靜態變數";
  public String name = "外部成員變數";
  
  private static void staticMethod1() {
    System.out.println("外部靜態方法");
  }

  private void method1() {
    System.out.println("外部方法");
  }

  public static class Static_class {
    String innclass_name = "內部變數";
    static String innclass_static_name = "內部靜態變數";
    
    public void innerMethod1() {
      System.out.println("內部方法");
    }

    public static void innerStaticMethod1() {
      System.out.println("內部靜態方法");
    }

    // 內部類的靜態方法
    public static void inner_static_memthod() {
      System.out.println("靜態內部類 靜態方法區塊");
      System.out.println("外部類別 靜態變數:" + static_name);
      staticMethod1();
      System.out.println("靜態內部類 靜態成員屬性innclass_static_name = " + innclass_static_name);
      innerStaticMethod1();
    }
  }

  public static void main(String[] args) {
    Test.Static_class.inner_static_memthod();
  }
}
{% endhighlight %}
```
靜態內部類 靜態方法區塊
外部類別 靜態變數:外部靜態變數
外部靜態方法
靜態內部類 靜態成員屬性innclass_static_name = 內部靜態變數
內部靜態方法
```

### 靜態內部類別的成員方法
以下無法存取:
- 外部類別成員屬性
- 外部類別成員方法

以下都可以存取
- 內部類別成員屬性
- 內部類別成員方法
- 外部類別靜態屬性
- 外部類別靜態方法
- 內部類別靜態屬性
- 內部類別靜態方法

{% highlight java linenos %}
public class Test {
  public static String static_name = "外部靜態變數";
  public String name = "外部成員變數";
  
  private static void staticMethod1() {
    System.out.println("外部靜態方法");
  }

  private void method1() {
    System.out.println("外部方法");
  }

  public static class Static_class {
    String innclass_name = "內部變數";
    static String innclass_static_name = "內部靜態變數";
    
    public void innerMethod1() {
      System.out.println("內部方法");
    }

    public static void innerStaticMethod1() {
      System.out.println("內部靜態方法");
    }

    public void inner_memthod() {
      System.out.println("靜態內部類 成員方法(非靜態)區塊");
      System.out.println("外部類別 靜態變數:" + static_name);
      staticMethod1();
      System.out.println("靜態內部類 成員屬性innclass_name = " + innclass_name);
      System.out.println("靜態內部類 靜態成員屬性innclass_static_name = " + innclass_static_name);
      innerMethod1();
      innerStaticMethod1();
      // method1();
      // System.out.println("外部類別 變數:" + name);
    }
  }

  public static void main(String[] args) {
    Test.Static_class staticClass = new Test.Static_class();
    staticClass.inner_memthod();
  }
}
{% endhighlight %}
```
靜態內部類 成員方法(非靜態)區塊
外部類別 靜態變數:外部靜態變數
外部靜態方法
靜態內部類 成員屬性innclass_name = 內部變數
靜態內部類 靜態成員屬性innclass_static_name = 內部靜態變數
內部方法
內部靜態方法
```

### 使用到靜態內部類別才會被建立

注意！當靜態內部類別要用到的時候，才會初始化。

初始化程式碼
{% highlight java linenos %}
public class Test {
  {
    System.out.println("外部匿名區塊");
  }

  static {
    System.out.println("外部靜態區塊");
  }

  Test() {
    System.out.println("外部建構子區塊");
  }

  public static void staticMethod1() {
    System.out.println("外部靜態方法");
  }
  public static class Static_class {
    Static_class() {
      System.out.println("內部類別建構子");
    }

    {
      System.out.println("內部匿名區塊");
    }

    static {
      System.out.println("內部靜態區塊");
    }

    public static void innerStaticMethod1() {
      System.out.println("內部靜態方法");
    }
  }
}
{% endhighlight %}

#### 未使用靜態內部類別
{% highlight java linenos %}
  public static void main(String[] args) {
    Test.staticMethod1();
  }
{% endhighlight %}
```
外部靜態區塊
外部靜態方法
```

由以上的結果可以發現，呼叫外部類別的靜態方法時，不會呼叫任何靜態內部類別區塊。

#### 使用靜態內部類別
{% highlight java linenos %}
  public static void main(String[] args) {
    Test.Static_class.innerStaticMethod1();
  }
{% endhighlight %}
```
外部靜態區塊
內部靜態區塊
內部靜態方法
```

由以上的結果可以發現，呼叫靜態內部類別的方法時，會呼叫靜態內部類別的「靜態區塊」。

#### 建立外部類別
{% highlight java linenos %}
  public static void main(String[] args) {
    Test test = new Test();
  }
{% endhighlight %}
```
外部靜態區塊
外部匿名區塊
外部建構子區塊
```

由以上的結果可以發現，外部類別建立的時候，完全不會建立靜態內部類別的任何代碼。

#### 建立靜態內部類別
{% highlight java linenos %}
  public static void main(String[] args) {
    Test.Static_class test = new Test.Static_class();
  }
{% endhighlight %}
```
外部靜態區塊
內部靜態區塊
內部匿名區塊
內部類別建構子
```

由以上的結果可以發現，外部類別只是名稱空間，建立靜態內部類別的時候，外部類別只會呼叫「靜態區塊」，外部的「匿名區塊」與外部「建構子」，完全沒被呼叫。

而靜態內部類別的「靜態區塊」與「匿名區塊」與「建構子」全呼叫。

## 靜態內部類別與jvm
static方法、static變數、static靜態內部類別，是在JVM啟動時，ClassLoader類別載入器就已經先丟進Method area(Metaspace)的靜態資料區中。

## 區域內部類別
- 定義在方法或代碼塊中的類別
- 不能使用 public、private 或 protected 修飾
- 只能被該方法或代碼塊內的代碼存取

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

[1]: {% link _pages/java/static.md %}
[2]: {% link _pages/java/constructor.md %}








