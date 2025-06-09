---
title: 介面切割原則
date: 2025-04-22
keywords: Java, Design patterns, Interface Segregation Principle
---
Prerequisites:

- [類別圖][1]
- [多型][2]
- [介面][3]

## 介面切割原則

英文是Interface Segregation Principle

我覺得稱為介面最小化原則比較適合。

下圖中有Interface1，ImplementA與ImplementB去實作Interface1

A與B類別會用到Interface1

![img]({{site.imgurl}}/pattern/segregation1.png)

Interface1
{% highlight java linenos %}
public interface Interface1 {
  public void method1();
  public void method2();
  public void method3();
  public void method4();
}
{% endhighlight %}

ImplementA
{% highlight java linenos %}
public class ImplementA implements Interface1 {
  @Override
  public void method1() {
  }

  @Override
  public void method2() {
  }

  @Override
  public void method3() {

  }

  @Override
  public void method4() {

  }
}
{% endhighlight %}

ImplementB
{% highlight java linenos %}
public class ImplementB implements Interface1 {
  @Override
  public void method1() {
  }

  @Override
  public void method2() {
  }

  @Override
  public void method3() {

  }

  @Override
  public void method4() {

  }
}
{% endhighlight %}

A類別用到Interface1，呼叫method1與method2與method3。
{% highlight java linenos %}
public class A {
  public void call1(Interface1 interface1) {
    interface1.method1();
  }
  public void call2(Interface1 interface1) {
    interface1.method2();
  }

  public void call3(Interface1 interface1) {
    interface1.method3();
  }
}
{% endhighlight %}

B類別用到Interfac1，呼叫method3與method4
{% highlight java linenos %}
public class B {
  public void call3(Interface1 interface1) {
    interface1.method3();
  }
  public void call4(Interface1 interface1) {
    interface1.method4();
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    A a = new A();
    B b = new B();
    ImplementA implementA = new ImplementA();
    ImplementB implementB = new ImplementB();
    a.call1(implementA);
    a.call2(implementA);
    a.call3(implementA);
    b.call3(implementB);
    b.call4(implementB);
  }
}
{% endhighlight %}

## 問題
以上程式碼是類別圖的實作，但會出現以下問題

### ImplementA與ImplementB實作Interface1全部方法

### Interface1的方法，A類別B類別不會全部用到
A類別不會用到ImplementA實作的method4()。

B類別不會用到ImplementB實作的method1()與method2()不會用到。

## 改善方法

A類別會用到method1(),method2(),method3()

B類別會用到method3(),method4()

1. A類別,B類別都會用到method3()，把method3()抽出來成為一個介面。
2. A類別還會用到method1()與method2()，把這二個抽出來成為一個介面。
3. B類別還會用到method4()，把method4()抽出來作為單獨的介面。

切割過的介面類別圖如下:

![img]({{site.imgurl}}/pattern/segregation2.png)

ImplementA實作Interface12與Interface3

ImplementB實作Interface4與Interface3

以下的類別圖加上A類別與B類別:

- A類別會用到Interface12與Interface3
- B類別會用到Interface4與Interface3

![img]({{site.imgurl}}/pattern/segregation3.png)

切割過的Interface程式碼如下:
{% highlight java linenos %}
public interface Interface12 {
  public void method1();
  public void method2();
}

public interface Interface3 {
  public void method3();
}

public interface Interface4 {
  public void method4();
}
{% endhighlight %}

ImplementA
{% highlight java linenos %}
public class ImplementA implements Interface12, Interface3 {

  @Override
  public void method1() {

  }

  @Override
  public void method2() {

  }

  @Override
  public void method3() {

  }
}
{% endhighlight %}

ImplementB
{% highlight java linenos %}
public class ImplementB implements Interface3, Interface4 {

  @Override
  public void method3() {

  }

  @Override
  public void method4() {

  }
}
{% endhighlight %}

A類別
{% highlight java linenos %}
public class A {
  public void call1(Interface12 interface12) {
    interface12.method1();
  }
  public void call2(Interface12 interface12) {
    interface12.method2();
  }
  public void call3(Interface3 interface3) {
    interface3.method3();
  }
}
{% endhighlight %}

B類別
{% highlight java linenos %}
public class B {
  public void call3(Interface3 interface3) {
    interface3.method3();
  }
  public void call4(Interface4 interface4) {
    interface4.method4();
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    A a = new A();
    B b = new B();
    ImplementA implementA = new ImplementA();
    ImplementB implementB = new ImplementB();
    a.call1(implementA);
    a.call2(implementA);
    a.call3(implementA);
    b.call3(implementB);
    b.call4(implementB);
  }
}
{% endhighlight %}

## View.OnClickListener
[View.OnClickListener](https://cs.android.com/android/platform/superproject/+/android15-qpr2-release:frameworks/base/core/java/android/view/View.java;l=31742?q=View.OnClickListener&ss=android%2Fplatform%2Fsuperproject)

可參考Android的View類別內的OnClickListener介面，只有一個抽象方法。

{% highlight java linenos %}
public interface OnClickListener {
  void onClick(View v);
}
{% endhighlight %}

[1]: {% link _pages/java/uml.md %}
[2]: {% link _pages/java/polymorphism.md %}
[3]: {% link _pages/java/interface.md %}