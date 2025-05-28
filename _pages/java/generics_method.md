---
title: 泛型方法
date: 2025-05-28
keywords: Java, Generics Method
---
Prerequisites:

- [介面][1]
- [泛型][2]
- [Wrap包裝類別][3]

泛型方法與泛型是不同的東西。

泛型是在類別名後面，自訂類型。

泛型方法是在傳回值前面，自訂類型。

泛型方法是給方法使用，不是類別使用。

## 自訂類型
在傳回值前面，自訂類型\<R, U, V\>，用尖括號包住自訂類型。

參數類型為自訂類型\<R, U, V\>。
{% highlight java linenos %}
class Dog {
  public <R, U, V> void eat(R r, U u, V v) {
    
  }
}
{% endhighlight %}

## 泛型中有泛型方法
下方程式，泛型方法eat的自訂類型X, Y, Z，不能跟泛型的自訂類型T, R, M英文字母一模一樣。
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  private T[] arr;
  private R r;
  private M m;
  public <X, Y, Z> void eat(X x, Y y, Z z) {

  }
}
{% endhighlight %}

## 呼叫泛型方法，透過參數知道類型
參數若是基本型態(short, int, long, double, float, boolean, char)會自動轉換為[包裝類別][3]，包裝類別有Short, Integer, Long, Double, Float, Boolean, Character。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Cat<Double, Boolean, Integer> cat = new Cat<>(12.5, true, 1);
    // 透過參數知道類型
    cat.eat("餅乾", 1, new ArrayList<Integer>());
    System.out.println("============================");
    cat.eat(new int[10], new ArrayList<String>(), 55.5);
  }
}
class Cat<T, R, M> {
  private T t;
  private T[] arr;
  private R r;
  private M m;

  public <X, Y, Z> void eat(X x, Y y, Z z) {
    System.out.println(x.getClass());
    System.out.println(y.getClass());
    System.out.println(z.getClass());
  }

  public Cat(T t, R r, M m) {
    this.t = t;
    this.r = r;
    this.m = m;
  }
}
{% endhighlight %}
```
class java.lang.String
class java.lang.Integer
class java.util.ArrayList
============================
class [I
class java.util.ArrayList
class java.lang.Double
```

由執行結果可以發現參數int轉成Integer，double轉成Double。

## 分辦泛型與泛型方法
sleep()是泛型，eat()是泛型方法。

sleep是使用泛型自訂類型T,R。

eat是使用泛型方法自訂類型X,Y,Z。

{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  private T[] arr;
  private R r;
  private M m;
  
  public T sleep(R r) {
    return t;
  }
  
  public <X, Y, Z> void eat(X x, Y y, Z z) {
    System.out.println(x.getClass());
    System.out.println(y.getClass());
    System.out.println(z.getClass());
  }
}
{% endhighlight %}

## 泛型方法可以用泛型的自訂類型
run泛型方法，T是用泛型自訂類型，K是用泛型方法自訂類型。
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  public <K> void run(T t, K k) {
    
  }
}
public class Test {
  public static void main(String[] args) {
    Cat<Double, Boolean, Integer> cat = new Cat<>(12.5, true, 1);
    cat.run(55.0, new ArrayList<Integer>());
  }
}    	
{% endhighlight %}
```
class java.lang.Double
class java.util.ArrayList
```
執行結果可以看出參數1類型是用Double，參數2類型是ArrayList。

[1]: {% link _pages/java/interface.md %}
[2]: {% link _pages/java/generics.md %}
[3]: {% link _pages/java/wrap.md %}