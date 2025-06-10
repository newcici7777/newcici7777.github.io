---
title: 繼承Memory Layout
date: 2025-04-18
keywords: Java, Extends Memory Layout
---
Prerequisites:

- [Java Memory Model][1]
- [建構子][2]

## 子類別、父類別、祖父類別
{% highlight java linenos %}
class GrandPa {
  String name = "GrandPa";
}
class Father extends GrandPa{
  String name = "Father";
}
class Child extends Father{
  String name = "Child";
}
{% endhighlight %}

## 建立子類別
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Child child = new Child();
  }
}
{% endhighlight %}

## 建立步驟
`new Child()`，發現子類別有父類別，依下方順序載入[metadata][3]到記憶體中。
1. GrandPa Metadata
2. Father Metadata
3. Child Metadata

![img]({{site.imgurl}}/java/extends_memory.png)

建立子類別
1. 建立子類別記憶體空間
2. 變數child指向記憶體空間。

![img]({{site.imgurl}}/java/extends_memory1.png)

根據[建構子][2]，預設會先呼叫祖父建構子，建立祖父物件。
1. 先在String Pool中建立GrandPa的文字
2. name指向String Pool中的記憶體位址。

![img]({{site.imgurl}}/java/extends_memory2.png)

呼叫父類別建構子，建立物件。
1. 先在String Pool中建立Father的文字
2. name指向String Pool中的記憶體位址。

![img]({{site.imgurl}}/java/extends_memory3.png)

建立Child物件。
1. 先在String Pool中建立Child的文字。
2. name指向String Pool中的記憶體位址。

![img]({{site.imgurl}}/java/extends_memory4.png)


[1]: {% link _pages/java/memory_model.md %}
[2]: {% link _pages/java/constructor.md %}#父類別與子類別建立順序
[3]: {% link _pages/java/metadata.md %}
