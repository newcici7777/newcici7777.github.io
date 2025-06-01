---
title: 靜態內部類別方法
date: 2025-05-30
keywords: Java, Static Inner class method
---
## 呼叫靜態內部類別.靜態方法
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter.StaticInner.innerSMethod1();
  }
}
class Outter {
  public static class StaticInner {
    public static void innerSMethod1() {
      System.out.println("呼叫靜態內部類別.靜態方法");
    }
  }
}
{% endhighlight %}
```
呼叫靜態內部類別.靜態方法
```

## 靜態內部類別.靜態方法
靜態方法只能存取靜態變數與靜態方法。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter.StaticInner.innerSMethod1();
  }
}
class Outter {
  public static String static_name = "外部靜態變數";
  private static void staticMethod1() {
    System.out.println("外部靜態方法");
  }

  public static class StaticInner {
    static String inner_sname = "內部靜態變數";
    public static void innerSMethod1() {
      System.out.println(static_name);
      System.out.println(inner_sname);
      staticMethod1();
      innerSMethod2();
    }
    // 內部類的靜態方法
    public static void innerSMethod2() {
      System.out.println("內部靜態方法");
    }
  }
}
{% endhighlight %}
```
外部靜態變數
內部靜態變數
外部靜態方法
內部靜態方法
```

## 靜態方法與Singleton
外部類別可以存取private內部類別，透過這個原理，由靜態內部類別建立外部類別物件。

1. 把外部類的建構子設為private。
2. 建立private靜態內部類別
3. 靜態內部類別，建立private靜態且final的屬性，屬性的類型是外部類別。
4. 靜態內部類別被建立時，屬性才會`new Outter()`
5. 建立一個public static的方法。
6. 取得靜態內部類別的私有靜態屬性。

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter outter1 = Outter.getInstance();
    System.out.println(outter1.hashCode());

    Outter outter2 = Outter.getInstance();
    System.out.println(outter2.hashCode());

    Outter outter3 = Outter.getInstance();
    System.out.println(outter3.hashCode());
  }
}
class Outter {
  // 1.
  private Outter() {
    System.out.println("建立物件");
  }
  // 2.
  private static class StaticInner {
    // 3.
    private static final Outter outter = new Outter(); // 4.
  }
  // 5.
  public static Outter getInstance() {
    // 6.
    return StaticInner.outter;
  }
}
{% endhighlight %}
```
建立物件
989110044
989110044
989110044
```

由執行結果可以發現，「建立物件」，只執行一次。

每一個物件的hashCode都是一模一樣的，代表物件只被建立一次。

### 靜態內部類別的成員方法
以下無法存取:
- 外部類別成員屬性、方法

其它都可以存取
- 內部類別成員屬性、方法
- 外部類別靜態屬性、靜態方法
- 內部類別靜態屬性、靜態方法

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter.StaticInner inner_s = Outter.getStaticInner();
    inner_s.call();
  }
}
class Outter {
  public String name = "外部變數";
  public static String static_name = "外部靜態變數";
  public void method() {
    System.out.println("外部方法");
  }
  private static void staticMethod() {
    System.out.println("外部靜態方法");
  }
  public static class StaticInner {
    String inner_name = "內部變數";
    static String inner_sname = "內部靜態變數";
    public void innerMethod() {
      System.out.println("內部方法");
    }
    // 內部類的靜態方法
    public static void innerSMethod1() {
      System.out.println("內部靜態方法");
    }
    public void call() {
      // 呼叫外部靜態變數
      System.out.println(static_name);
      // 呼叫外部靜態方法
      staticMethod();
      // 內部的變數、方法可以呼叫
      System.out.println(inner_name);
      innerMethod();
      // 內部的靜態變數、靜態方法可以呼叫
      System.out.println(inner_sname);
      innerSMethod1();
    }
  }
  public static StaticInner getStaticInner() {
    return new StaticInner();
  }
}
{% endhighlight %}
```
外部靜態變數
外部靜態方法
內部變數
內部方法
內部靜態變數
內部靜態方法
```
