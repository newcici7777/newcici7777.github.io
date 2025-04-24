---
title: 建構子
date: 2025-04-17
keywords: Java, constructor
---
## 匿名程式碼區塊
英文是Anonymous code blocks

{}花括號裡面的內容，就是匿名程式碼區塊
```
{
  // 花括號裡面的內容
}
```

## 先執行匿名程式碼區塊
建立物件時，會先執行匿名稱式碼區塊，再執行建構子
{% highlight java linenos %}
public class Test {
  {
    System.out.println("匿名區塊");
  }

  Test() {
    System.out.println("建構子區塊");
  }

  public static void main(String[] args) {
    Test test = new Test();
  }
}
{% endhighlight %}
```
匿名區塊
建構子區塊
```

## 父類別沒有無參數建構子
以下父類別沒有無參數建構子`Parent() {}`
{% highlight java linenos %}
public class Parent {
  String name;
  int age;
  Parent(String name) {
    this.name = name;
  }
  Parent(String name, int age) {
    this.name = name;
    this.age = age;
  }
}
{% endhighlight %}

子類別繼承父類別，會產生錯誤。
{% highlight java linenos %}
public class Child extends Parent{
}
{% endhighlight %}

原因是，父類別沒有無參數建構子`Parent() {}`，編譯器在編譯時，會添加super()在子類別的建構子中。  
以下是編譯器自動增加的:
{% highlight java linenos %}
public class Child extends Parent{
  Child() {
    super();  //父類別沒有無參數建構子，所以產生error
  }
}
{% endhighlight %}

## 解決辦法

### 子類別呼叫任何一個有參數的父建構子
在 Java 中，當一個類別（Child）繼承自另一個類別（Parent），而父類別沒有無參數的建構函式（default constructor）時，子類別必須明確地呼叫父類別的某個建構函式（使用 super(...)）。

不管如何，子類別至少呼叫任何一個有參數的父建構子。
{% highlight java linenos %}
public class Child extends Parent{
  Child(String name) {
    super(name);
  }
}
{% endhighlight %}

### 父類別加上無參數建構子
父類別加上無參數建構子
{% highlight java linenos %}
public class Parent {
  String name;
  int age;
  Parent(String name) {
    this.name = name;
  }
  Parent(String name, int age) {
    this.name = name;
    this.age = age;
  }
  Parent() {}
}
{% endhighlight %}

子類別就無需呼叫父類別的無參數建構子，編譯器會自動產生在程式碼。
{% highlight java linenos %}
public class Child extends Parent{
}
{% endhighlight %}