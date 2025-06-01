---
title: 靜態內部類別
date: 2025-05-30
keywords: Java, Static Inner class
---
Prerequisites:

- [Static][1]
- [建構子][2]

## 使用靜態內部類別
什麼是靜態內部類別？其實也是一個類別，只是外部類別變成名稱空間，呼叫靜態內部類別的靜態方法，這樣呼叫:
{% highlight java linenos %}
外部類別.靜態內部類別.靜態方法();
{% endhighlight %}

呼叫靜態方法時，類別名也變成名稱空間，這樣呼叫:
{% highlight java linenos %}
類別.靜態方法();
{% endhighlight %}

以上呼叫的方法如出一徹。

## 靜態內部類別是一個類別
靜態內部類別是一個類別，所以類別該有的存取權限(public、private、protected、default)、屬性、方法、建構子、匿名程式碼區塊、靜態程式碼區塊、靜態變數、靜態方法…等等，靜態內部類別也有。

## static關鍵字
在內部類前面加上static的關鍵字就變成靜態內部類別。
{% highlight java linenos %}
class Outter {
  public static class StaticInner {
  }
}
{% endhighlight %}

## 建立靜態內部類別
### 使用new
語法
```
外部類別.內部類別 變數 = new 外部類別.內部類別()
```

注意！靜態內部類別建立的方式為「內部類別」差距很大，不相同。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter.StaticInner inner_s = new Outter.StaticInner();
  }
}
class Outter {
  public static class StaticInner {
  }
}
{% endhighlight %}

### 外部類別提供public static方法
因為靜態內部類別，是屬於類別，所以使用static方法取得靜態內部類別。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter.StaticInner inner_s = Outter.getStaticInner();
  }
}
class Outter {
  public static class StaticInner {
  }
  public static StaticInner getStaticInner() {
    return new StaticInner();
  }
}
{% endhighlight %}

## 外部類別提供public static方法存取private
外部類別提供public static方法，去存取private靜態內部類別。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(Outter.getInnerStr());
  }
}
class Outter {
  // 私有靜態內部類別
  private static class StaticInner {
    // 私有靜態屬性
    private static String str = "私有內部屬性";
  }
  // 外部公有靜態方法
  public static String getInnerStr() {
    // 取得private靜態內部類別.private靜態屬性
    return StaticInner.str;
  }
}
{% endhighlight %}
```
私有內部屬性
```

## 靜態內部類別建構子
靜態內部類別就是類別，所以一定有建構子。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Outter.StaticInner inner_s = Outter.getStaticInner();
  }
}
class Outter {
  public static class StaticInner {
    StaticInner() {
      System.out.println("建構子");
    }
  }
  public static StaticInner getStaticInner() {
    return new StaticInner();
  }
}
{% endhighlight %}
```
建構子
```

## 使用到靜態內部類別才會被建立
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

### 未使用靜態內部類別
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

### 使用靜態內部類別
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

### 建立外部類別
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

### 建立靜態內部類別
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


[1]: {% link _pages/java/static.md %}
[2]: {% link _pages/java/constructor.md %}
