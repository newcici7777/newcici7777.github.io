---
title: 泛型介面
date: 2025-05-06
keywords: Java, Generics Interface
---
泛型介面主要用途在於檢查介面的類型是否正確。

## 語法
interface 名稱後面有一對`<>`尖括號，尖括號包著的是泛型T
{% highlight java linenos %}
interface 名稱<T> {
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