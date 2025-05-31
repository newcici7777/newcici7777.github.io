---
title: 建構子
date: 2025-04-17
keywords: Java, constructor
---
## 子類別預設會繼承父類別無參數建構子
以下看似什麼都沒有做。
{% highlight java linenos %}
class Father {}
class Child extends Father {}
{% endhighlight %}

但實際上子類別的建構子，會先呼叫父類別無參數建構子，建立完父類別，才建立自己。
{% highlight java linenos %}
class Father {
  public Father() {  }
}
class Child extends Father{
  public Child() {
    super();  // 先呼叫父類別無參數建構子，建立父類別
    // 建立完父類別，才建立自己
  }
}
{% endhighlight %}

如果父類別沒有無參數建構子，會產生錯誤。

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
public class Child extends Parent {}
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

## 父類別與子類別建立順序
建立Child
{% highlight java linenos %}
public static void main(String[] args) {
  Child child = new Child();
}
{% endhighlight %}

{% highlight java linenos %}
class Grandpa {
  public Grandpa() {
    // 3.建立Grandpa
    System.out.println("Grandpa無參數建構子");
  }
}
class Father extends Grandpa{
  public Father() {
    super();  // 2.先呼叫父類別無參數建構子
    // 4. 建立Father
    System.out.println("Father無參數建構子");  
  }
}
class Child extends Father{
  public Child() {
    super();  // 1.先呼叫父類別無參數建構子
    // 5. 建立Child
    System.out.println("Child無參數建構子");
  }
}
{% endhighlight %}
```
Grandpa無參數建構子
Father無參數建構子
Child無參數建構子
```

## 子類別有參數建構子呼叫順序
執行以下程式碼，執行順序是什麼？
{% highlight java linenos %}
public static void main(String[] args) {
  Child child = new Child("Momo");
}
{% endhighlight %}

{% highlight java linenos %}
class Grandpa {
  public Grandpa() {
    // 3.建立Grandpa
    System.out.println("Grandpa無參數建構子");
  }
}

class Father extends Grandpa{
  public Father() {
    super();  // 2.先呼叫父類別無參數建構子
    // 4. 建立Father
    System.out.println("Father無參數建構子");
  }
}

class Child extends Father{
  public Child() {
    super();
    System.out.println("Child無參數建構子");
  }
  public Child(String name) {
    super();  // 1.先呼叫父類別無參數建構子
    // 5. 建立Child
    System.out.println("Child有參數建構子");
  }
}
{% endhighlight %}
```
Grandpa無參數建構子
Father無參數建構子
Child有參數建構子
```

注意！呼叫有參數的建構子，不會再去呼叫無參數建構子。

## this()呼叫無參數建構子
執行以下程式碼，執行順序是什麼？
{% highlight java linenos %}
public static void main(String[] args) {
  Child child = new Child("Momo");
}
{% endhighlight %}

{% highlight java linenos %}
class Grandpa {
  public Grandpa() {
    // 4.建立Grandpa
    System.out.println("Grandpa無參數建構子");
  }
}
class Father extends Grandpa{
  public Father() {
    super();  // 3.先呼叫父類別無參數建構子
    // 5. 建立Father
    System.out.println("Father無參數建構子");
  }
}
class Child extends Father{
  public Child() {
    super();// 2.先呼叫父類別無參數建構子
    // 6. 
    System.out.println("Child無參數建構子");
  }
  public Child(String name) {
    this();  // 1.呼叫無參數建構子
    // 7. 這裡執行完有參數建構子，才建立Child
    System.out.println("Child有參數建構子");
  }
}
{% endhighlight %}
```
Grandpa無參數建構子
Father無參數建構子
Child無參數建構子
Child有參數建構子
```
## super()與this()只能擇一，且只能放第一行
以下編譯錯誤
{% highlight java linenos %}
class Child extends Father{
  public Child() {
    super();
  }
  public Child(String name) {
    this();  
    super();
    // this與super只能擇一
    // 且只能在第一行，super不能放在第二行
  }
{% endhighlight %}

## 最後一定會呼叫Object()
所有建構子，最後一定會呼叫Object()建構子，因為Object是所有類別的父類別。

滑鼠對著子類別名稱，按下ctrl + h，出現以下階層圖，可以由此圖知道Object是所有類別的父類別。

![img]({{site.imgurl}}/java/hierarchy2.png)

以下為呼叫過程圖，最後會在Heap中建立一個記憶體空間0x00055，包含每個父類別的「屬性」，而父類別「們」的方法則存放在[method Area方法區][1]的[vtable][2]中。
![img]({{site.imgurl}}/java/super_hierarchy.png)

## 匿名程式碼區塊
英文是Anonymous code blocks

{}花括號裡面的內容，就是匿名程式碼區塊
```
{
  // 花括號裡面的內容
}
```

## 先執行匿名程式碼區塊，再執行建構子
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

[1]: {% link _pages/java/memory_model.md %}#物件建立過程
[2]: {% link _pages/java/polymorphism.md %}#vtable