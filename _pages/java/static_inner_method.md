---
title: 靜態內部類別方法
date: 2025-05-30
keywords: Java, Static Inner class method
---
## 靜態內部類別方法
只能存取(外部與內部)靜態變數與靜態方法
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
      System.out.println("外部 靜態變數:" + static_name);
      staticMethod1();
      System.out.println("靜態成員屬性innclass_static_name = " + innclass_static_name);
      innerStaticMethod1();
    }
  }
  public static void main(String[] args) {
    Test.Static_class.inner_static_memthod();
  }
}
{% endhighlight %}
```
外部 靜態變數:外部靜態變數
外部靜態方法
靜態成員屬性innclass_static_name = 內部靜態變數
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
      System.out.println("外部類別 靜態變數:" + static_name);
      staticMethod1();
      System.out.println("成員屬性innclass_name = " + innclass_name);
      System.out.println("靜態成員屬性innclass_static_name = " + innclass_static_name);
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
外部類別 靜態變數:外部靜態變數
外部靜態方法
成員屬性innclass_name = 內部變數
靜態成員屬性innclass_static_name = 內部靜態變數
內部方法
內部靜態方法
```
