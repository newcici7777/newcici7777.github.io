---
title: 泛型介面
date: 2025-05-06
keywords: Java, Generics Interface
---
Prerequisites:

- [介面][1]
- [泛型][2]

## 語法
在類別名後面，用尖括號包住\<自訂類型\>。

自訂類型的名稱可用T, R, K, V 任意一個大寫字母。

{% highlight java linenos %}
interface 介面名<T, R, M> {
}
{% endhighlight %}

## 抽象方法
介面中的方法都是抽象。
{% highlight java linenos %}
interface IMove<T, R, M> {
  R doMove(M m);
  void move(T t1, T t2, R r1, M m1);
}
{% endhighlight %}

## 繼承時要指定泛型類型
以下程式碼，IFly繼承IMove時，要指定泛型類型Integer, String, Object，才能繼承。
{% highlight java linenos %}
interface IMove<T, R, M> {
  R doMove(M m);
  void move(T t1, T t2, R r1, M m1);
}

interface IFly extends IMove<Integer, String, Object> {
  
}
{% endhighlight %}

Duck實作IFly繼承下來的抽象方法，會自動替換類型，T, R, M 替換成 Integer, String, Object。

實作抽象方法。

![img]({{site.imgurl}}/java/interface_generic1.png)

選取要實作的抽象方法，按ok。

![img]({{site.imgurl}}/java/interface_generic2.png)

方法中的類型被自動填寫。

![img]({{site.imgurl}}/java/interface_generic3.png)

{% highlight java linenos %}
class Duck implements IFly {

  @Override
  public String doMove(Object o) {
    return null;
  }

  @Override
  public void move(Integer t1, Integer t2, String r1, Object m1) {

  }
}
{% endhighlight %}

## 實作時要指定泛型類型
泛型介面
{% highlight java linenos %}
interface ISwim<T, R, M> {
  void swim(T t1, T t2, R r1, M m1);
}
{% endhighlight %}

實作時要指定介面的泛型類型。
{% highlight java linenos %}
class Fish implements ISwim<Object, Character, String> {

  @Override
  public void swim(Object t1, Object t2, Character r1, String m1) {
    
  }
}
{% endhighlight %}

## 沒指定泛型類型，預設用Object
潛水艇(Submarine)類別實作ISwim沒指定泛型類型，預設泛型類型為Object，所以swim方法的參數類型全部都是Object。
{% highlight java linenos %}
class Submarine implements ISwim {
  @Override
  public void swim(Object t1, Object t2, Object r1, Object m1) {
    
  }
}
{% endhighlight %}

上面的程式碼與下面的程式碼是相同意思。

{% highlight java linenos %}
class Submarine implements ISwim<Object, Object, Object> {
  @Override
  public void swim(Object t1, Object t2, Object r1, Object m1) {
    
  }
}
{% endhighlight %}

## 不能使用泛型的情況
不能用靜態static

--------------------------------------
以下為其它筆記。

泛型介面主要用途在於檢查介面的類型是否正確。

## 語法
interface 介面名後面有一對`<>`尖括號，尖括號包著的是泛型T。
{% highlight java linenos %}
interface 介面名<T> {
}
{% endhighlight %}

T，代表什麼類型都可以，除了基本類型(int, float, double ...)之外。
{% highlight java linenos %}
interface Listen<T> {
  void listen(T obj);
}
{% endhighlight %}

## 使用泛型介面
類別實作泛型介面，要指定型別。

Listen泛型介面
{% highlight java linenos %}
interface Listen<T> {
  void listen(T obj);
}
{% endhighlight %}

Piano
{% highlight java linenos %}
class Piano {
  void play() {
    System.out.println("Piano play");
  }
}
{% endhighlight %}

Guitar
{% highlight java linenos %}
class Guitar {
  void play() {
    System.out.println("Guitar play");
  }
}
{% endhighlight %}

一般類別實作泛型介面要指定類型。
{% highlight java linenos %}
class Student implements Listen<Piano> {
  @Override
  public void listen(Piano obj) {
    obj.play();
  }
}
{% endhighlight %}

一般類別實作泛型介面要指定類型。
{% highlight java linenos %}
class Teacher implements Listen<Guitar> {
  @Override
  public void listen(Guitar obj) {
    obj.play();
  }
}
{% endhighlight %}

測試
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Student student = new Student();
    // 只能放Piano，不然會出錯
    student.listen(new Piano());

    Teacher teacher = new Teacher();
    teacher.listen(new Guitar());
  }
}
{% endhighlight %}
```
Piano play
Guitar play
```

[1]: {% link _pages/java/interface.md %}
[2]: {% link _pages/java/generics.md %}