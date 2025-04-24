---
title: Static
date: 2025-04-19
keywords: Java, static
---
## 靜態變數
在類別中，除了有成員變數之外，還有一個靜態變數。

什麼是靜態變數？靜態變數可以當作共享資源，所有物件都可以共享這個資源。

使用方法:  
```
類別名.靜態變數
```
{% highlight java linenos %}
public class Test {
  public static String static_name = "靜態變數";
  public static void main(String[] args) {
    System.out.println(Test.static_name);
  }
}
{% endhighlight %}
```
靜態變數
```

## 靜態方法
靜態方法使用方法有下列二種

使用方法:  
```
類別名.靜態方法()
```
{% highlight java linenos %}
public class Test {
  public static void staticMethod1() {
    System.out.println("靜態方法");
  }
  public static void main(String[] args) {
    Test.staticMethod1();
  }
}
{% endhighlight %}
```
靜態方法
```
## 靜態區塊
語法
{% highlight java linenos %}
  static {
    System.out.println("靜態區塊");
  }
{% endhighlight %}

[建構子][1]的文章中，提到建立物件會先呼叫「匿名區塊」，才呼叫建構子。

那如果加上靜態區塊？誰先執行？答案是靜態區塊先執行，然後才是匿名區塊，最後是建構子

{% highlight java linenos %}
public class Test {
  // 靜態區塊
  static {
    System.out.println("靜態區塊");
  }

  {
    System.out.println("匿名區塊");
  }

  Test() {
    System.out.println("建構子區塊");
  }

  public static void main(String[] args) {
    Test test1 = new Test();
  }
}
{% endhighlight %}
```
靜態區塊
匿名區塊
建構子區塊
```

## 靜態只產生一次
把前一個程式碼的main主程式改成下方，會發現靜態區塊只產生一次。
{% highlight java linenos %}
  public static void main(String[] args) {
    Test test1 = new Test();
    Test test2 = new Test();
  }
{% endhighlight %}
```
靜態區塊
匿名區塊
建構子區塊
匿名區塊
建構子區塊
```

由以上結果可知，執行2次new建立物件，但「靜態區塊」只會產生一次，不管建立多少次物件，靜態變數、靜態方法、靜態區塊，只會產生一次。

## static與jvm
static方法、static變數、static靜態內部類別，是在JVM啟動時，ClassLoader類別載入器就已經先丟進Method area(Metaspace)的靜態資料區中，若靜態變數的類型是物件(如String)，就會把「靜態變數」放在jvm的stack中，而變數指向的「物件」則放入jvm的heap(堆)的區域中。

## 靜態方法只能存取靜態變數、靜態方法
靜態方法沒辦法存取成員變數與成員方法

{% highlight java linenos %}
public class StaticExample {
  public static String static_name = "static_name";
  public String name = "name";
  public static void staticMethod1() {
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void staticMethod2() {
    System.out.println("呼叫 staticMethod2()");
  }

  public static void main(String[] args) {
    StaticExample.staticMethod1();
  }
}
{% endhighlight %}
```
呼叫靜態方法:
呼叫 staticMethod2()
印出靜態變數:static_name
```

## 成員方法可以存取靜態變數、靜態方法
{% highlight java linenos %}
public class StaticExample {
  public static String static_name = "static_name";
  public String name = "name";

  public static void staticMethod1() {
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void staticMethod2() {
    System.out.println("呼叫 staticMethod2()");
  }

  public void method1() {
    System.out.println("印出成員變數:" + name);
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void main(String[] args) {
    StaticExample example = new StaticExample();
    example.method1();
  }
}
{% endhighlight %}
```
印出成員變數:name
呼叫靜態方法:
呼叫 staticMethod2()
印出靜態變數:static_name
```

## main主程式也是靜態方法
main主程式也是靜態方法，可以直接存取類別的靜態方法。

{% highlight java linenos %}
public class StaticExample {
  public static String static_name = "static_name";
  public String name = "name";

  public static void staticMethod1() {
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void staticMethod2() {
    System.out.println("呼叫 staticMethod2()");
  }

  public void method1() {
    System.out.println("印出成員變數:" + name);
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void main(String[] args) {
    staticMethod1();
  }
}
{% endhighlight %}
```
呼叫靜態方法:
呼叫 staticMethod2()
印出靜態變數:static_name
```

[1]: {% link _pages/java/constructor.md %}


