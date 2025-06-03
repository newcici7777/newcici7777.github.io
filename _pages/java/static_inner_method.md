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
