---
title: 繼承 Tricky
date: 2025-05-31
keywords: Java, Extends Tricky
---
Prerequisites:

- [建構子][1]

## 父類別與祖父類別都有相同屬性
父類別與祖父類別都有hobby。
{% highlight java linenos %}
class GrandPa {
  String name = "GrandPa";
  String hobby = "Swimming";
  int age = 88;
}
class Father extends GrandPa{
  String name = "Father";
  String hobby = "Running";
  private int age = 50;
}
class Child extends Father{
  String name = "Child";
}
{% endhighlight %}

請問呼叫子類別hobby，會印出那個hobby？
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Child child = new Child();
    System.out.println(child.hobby);
  }
}
{% endhighlight %}
```
Running
```
從執行結果，子類會先從父類別搜尋有沒有hobby這個屬性，若有的話，就傳回，不再去搜尋祖父類別。

## 父類別屬性是private，祖父類別不是private
上面的程式碼中，父類別的age是private，但祖父類別的age不是private，請問子類別使用age，會發生什麼事？
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Child child = new Child();
    System.out.println(child.age);
  }
}
{% endhighlight %}

執行結果是無法存取age屬性，因為先從父類別搜尋屬性，有搜尋到age屬性，但age是private，就傳回無法存取。因為有搜尋到屬性，即便是private，就不會再往祖父類別搜尋。


[1]: {% link _pages/java/constructor.md %}#父類別與子類別建立順序