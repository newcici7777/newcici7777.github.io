---
title: 介面轉接器
date: 2025-04-28
keywords: Java, Design patterns, Class Adapter Pattern
---
Prerequisites:

- [介面切割原則][1]
- [匿名類別][2]

## 原理
在認識此模式之前，一定要先了解介面切割原則，二者為互斥。

先前是介面最小原則，本篇是介面不用切割了，使用一個抽象的Adapter，把所有介面的所有方法都「空」實作。

繼承Adapter的類別，要用到那些介面的方法，再去覆寫，完全大大改善「介面切割原則」會產生許多介面與實作的類別數量問題。

## 類別圖
![img]({{site.imgurl}}/pattern/class_adapter.png)

## Interface
程式碼如下
{% highlight java linenos %}
public interface Interface1 {
  void method1();
  void method2();
  void method3();
  void method4();
}
{% endhighlight %}

## Adapter
抽象的Adapter，把所有介面的所有方法都「空」實作。
{% highlight java linenos %}
public abstract class Adapter1 implements Interface1{
  @Override
  public void method1() {}

  @Override
  public void method2() {}

  @Override
  public void method3() {}

  @Override
  public void method4() {}
}
{% endhighlight %}

## 繼承Adapter的類別  
A類別，把需要覆寫的方法寫一寫。
{% highlight java linenos %}
public class A extends Adapter1{
  @Override
  public void method1() {
    System.out.println("A method1");
  }

  @Override
  public void method2() {
    System.out.println("A method2");
  }

  @Override
  public void method3() {
    System.out.println("A method3");
  }
}
{% endhighlight %}

B類別，把需要覆寫的方法寫一寫。
{% highlight java linenos %}
public class B extends Adapter1{
  @Override
  public void method3() {
    System.out.println("B method3");
  }
  @Override
  public void method4() {
    System.out.println("B method4");
  }
}
{% endhighlight %}

## 測試
main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    A a = new A();
    a.method1();
    a.method2();
    a.method3();
    B b = new B();
    b.method3();
    b.method4();
  }
}
{% endhighlight %}
```
A method1
A method2
A method3
B method3
B method4
```

## 使用匿名類別實作
這種方式是最多人使用，把需要覆寫的方法寫一寫。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Interface1 test = new Adapter1() {
      @Override
      public void method1() {
        System.out.println("A method1");
      }

      @Override
      public void method2() {
        System.out.println("A method2");
      }

      @Override
      public void method3() {
        System.out.println("A method3");
      }
    };
    test.method1();
    test.method2();
    test.method3();
  }
}
{% endhighlight %}
```
A method1
A method2
A method3
```

## AnimationListener與AnimationListenerAdapter

[source code](https://cs.android.com/android/platform/superproject/+/android15-qpr2-release:frameworks/base/core/java/android/view/animation/Animation.java;l=1340?q=AnimationListen&ss=android%2Fplatform%2Fsuperproject)

AnimationListener
{% highlight java linenos %}
public static interface AnimationListener {
  void onAnimationStart(Animation animation);
  void onAnimationEnd(Animation animation);
  void onAnimationRepeat(Animation animation);
}
{% endhighlight %}

AnimationListenerAdapter空實作AnimationListener所有方法
{% highlight java linenos %}
public class AnimationListenerAdapter implements AnimationListener {
    @Override
    public void onAnimationStart(Animation animation) {}

    @Override
    public void onAnimationEnd(Animation animation) {}

    @Override
    public void onAnimationRepeat(Animation animation) {}
}
{% endhighlight %}

可透過匿名類別只實作Adapter1個方法
{% highlight java linenos %}
new AnimationListenerAdapter() {
  @Override
  public void onAnimationStart(Animation animation) {
    // 只實作1個方法
  }
}
{% endhighlight %}

[1]: {% link _pages/design_pattern/segregation.md %}
[2]: {% link _pages/java/anonymous_class.md %}