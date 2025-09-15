---
title: 泛型方法
date: 2025-05-28
keywords: Java, Generics Method
---
Prerequisites:

- [介面][1]
- [泛型][2]
- [Wrap包裝類別][3]

泛型方法與泛型類別是不同的東西。

泛型是在類別名後面，自訂類型。

泛型方法是在傳回值前面，自訂類型。

泛型方法是給方法使用，不是類別使用。

## 自訂泛型方法類型
在方法傳回值前面，自訂類型\<R, U, V\>，用尖括號包住自訂類型。

參數類型為自訂類型\<R, U, V\>。
{% highlight java linenos %}
class Dog {
  public <R, U, V> void eat(R r, U u, V v) {
    
  }
}
{% endhighlight %}

## 泛型類別與泛型方法的差別
下面程式碼，`Obj<T>`，這邊的T是泛型類別，建立泛型類別時，透過尖括號`<類型>`，設定T的類型。<br>

而method1()使用的是泛型類別的尖括號`<Integer>`的類型，參數是Integer。<br>

而method2()方法傳回值類型前有 <T>，使用的是泛型方法的類型，根據傳進來的參數類型決定T是什麼類型。<br>
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Obj<Integer> obj = new Obj<>();
    obj.method1(100);
    obj.method2("Hello world!");
    obj.method2(10000.0);
    obj.method2('C');
  }
}

class Obj<T> {
  private T t;
  public void method1(T parm1) {
    this.t = parm1;
    System.out.println(t);
  }

  public <T> void method2(T parm1) {
    System.out.println(parm1);
  }
}
{% endhighlight %}
```
100
Hello world!
10000.0
C
```

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

## 泛型方法可以用泛型類別的自訂類型
run泛型方法，T是用泛型類別自訂類型，K是用泛型方法自訂類型。
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