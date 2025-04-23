---
title: HAS-A比IS-A更好
date: 2025-04-23
keywords: Java, Design patterns, Liskov Substitution Principle, HAS-A, IS-A
---
Prerequisites:

- [類別圖][1]
- [多型][2]

## 有一個比是一個更好

HAS-A是「有一個」，IS-A是「是一個」。

HAS-A意思是多用依賴、聚合、組合，少用繼承IS-A。

所謂的依賴、聚合、組合，請參考類別圖的解釋，才能繼續看以下文章。

里氏替換原則，英文是Liskov Substitution Principle

無法一言以蔽之，請看以下的內容。

下圖，A類別是父類別，B類別是A類別的子類別。

![img]({{site.imgurl}}/pattern/lsp1.png)

程式碼中，B類別覆寫A類別的method1()方法
{% highlight java linenos %}
class A {
  void method1() {
    System.out.println("A method1");
  }
}

class B extends A{
  @Override
  void method1() {
    System.out.println("B method1");
  }
}
{% endhighlight %}

多型的時候，父類別A想呼叫的是A類別自己的method1()，但A不知道B已經覆寫method1()，執行時是呼叫B類別的method1()
{% highlight java linenos %}
  public static void main(String[] args) {
    A a = new B();
    a.method1();
  }
{% endhighlight %}
```
B method1
```

由以上的程式碼可以發現，子類別覆寫到跟父類別相同的方法，就會影嚮預期，也許其它程式碼想呼叫的是父類別的方法，但在參數為多型的時候，卻呼叫到子類別覆寫的方法。

## 解決方式
降低耦合，使用「有一個」HAS-A，而非繼承IS-A。

HAS-A意思是多用依賴、聚合、組合。

B類別想使用A類別的method1()，透過依賴使用A類別，而不是使用繼承來使用A類別的method1();

![img]({{site.imgurl}}/pattern/lsp2.png)

B類別改成依賴的方式呼叫A類別的method1()
{% highlight java linenos %}
class A {
  void method1() {
    System.out.println("A method1");
  }
}

class B {
  void method1() {
    System.out.println("B method1");
  }
  void callA_method1(A a) {
    a.method1();
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public static void main(String[] args) {
  B b = new B();
  b.callA_method1(new A());
}
{% endhighlight %}
```
A method1
```

[1]: {% link _pages/java/uml.md %}
[2]: {% link _pages/java/polymorphism.md %}