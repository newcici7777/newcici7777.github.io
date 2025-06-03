---
title: super與this
date: 2025-05-29
keywords: Java, super, this
---
## super的意義
可以與子類別「分開」執行父類別程式碼。

## 使用父類別的方法、屬性
只能使用父類別不是private的方法與屬性。

語法
```
super.方法()
super.屬性
```

## super()建構子
只能在子類別的建構子使用，而且只能放在第一行。

呼叫super()的意思是，呼叫父類別的無參數建構子。

呼叫super(參數)的意思是，呼叫父類別的有參數建構子。

## 當只有一個父類別屬性，this與super取得屬性是相同
父類別的name
{% highlight java linenos %}
class Father {
  String name = "Father";
  String hobby = "Running";
  private int age = 50;
  void method1() {
  }
}
{% endhighlight %}

子類別沒有覆寫name，在method1印出name
{% highlight java linenos %}
class Child extends Father {
  @Override
  void method1() {
    // 以下三種呼叫都是相同的父類別的name
    System.out.println(super.name);
    System.out.println(this.name);
    System.out.println(name);
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Father obj = new Child();
    obj.method1();
  }
}
{% endhighlight %}
```
Father
Father
Father
```

## super.方法()直接去父類別找方法
在子類別中，使用super.方法()，跳過子類別，直接去父類別去找有沒有這個方法，找到就呼叫，找不到就找祖父類別，再找不到也不會回來找子類別有沒有這個方法。
{% highlight java linenos %}
class Child extends Father {
  @Override
  void method1() {
    System.out.println(super.name);
    System.out.println(this.name);
    System.out.println(name);
  }
  
  void method2() {
    // 執行父類別的method1()
    super.method1();
  }
}
{% endhighlight %}

## this
代表new出來的物件。
```
類別 this = new 類別();

this.方法()
this.屬性
```

## 繼承中this.方法()與this.屬性
父類有method1與method2，與hobby屬性
{% highlight java linenos %}
class Father {
  String name = "Father";
  String hobby = "Running";
  private int age = 50;
  void method1() {
    System.out.println("Father method1");
  }
  void method2() {
    System.out.println("Father method2");
  }
}
{% endhighlight %}

子類別有method1(覆寫)、method3，請問，在子類別呼叫this.method2()與this.hobby，會呼叫誰的？
{% highlight java linenos %}
class Child extends Father {
  int count = 0;
  @Override
  void method1() {
    System.out.println("Child Method1");
  }
  void method3() {
    // 用this呼叫非子類別的method2()
    this.method2();
    // 用this呼叫非子類別的hobby屬性
    System.out.println(this.hobby);
  }
}
{% endhighlight %}
```
Father method2
Running
```

執行結果可以發現，this若本身的類別找不到，就會往父類別找有沒有方法與屬性。

## this變數、方法尋找方式
先找本身的類別有沒有，本身的類別找不到，再往父類別找，父類別再找不到，繼續往上找，找到就停下來，不會再往上找。

## super與this

|關鍵字|方法與變數|
|:---|:---------|
|this|先找本身類別，找不到找父類別，再找不到找祖父類別。|
|super|直接找父類別，找到就停止不往祖父類別找，不會回頭找子類別有沒有。|

|關鍵字|建構子|
|:---|:---------|
|this()|只能在建構子中使用，放在第一行，呼叫本身類別建構子，根據建構子參數。|
|super()|只能在子類別建構子中使用，放在第一行，呼叫父類別建構子，根據建構子參數。|