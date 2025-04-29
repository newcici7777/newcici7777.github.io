---
title: 合成模式(樹狀結構)
date: 2025-04-29
keywords: Java, Design patterns, Composite Pattern
---
樹狀結構的物件。

![img]({{site.imgurl}}/pattern/composite.png)

Componet為抽象類別，裡面有name的屬性，有add()、remove()，還有一個抽象方法print()，繼承的子類別一定要覆寫print()。

Leaf為樹狀結構最尾端的節點，在他的下面不會再有任何節點，所以不允許使用add()與remove()的方法。

Composite下面一定會有子節點，子節點可以是Composite也可以是Leaf，可以使用add()與remove()的方法增加子節點。

接下來我們要寫一個學校的樹狀結構

大學University -> College學院 -> Department系

XX大學 -> 電資學院 -> 通訊系

XX大學 -> 商學院 -> 會計系

![img]({{site.imgurl}}/pattern/composite2.png)

Component
{% highlight java linenos %}
public abstract class Component {
  protected String name;

  public void add(Component component) {}

  public void remove(Component component) {}

  public abstract void print();
}
{% endhighlight %}

University
{% highlight java linenos %}
public class University extends Component{
  List<Component> componentList = new ArrayList<>();

  public University(String name) {
    super();
    this.name = name;
  }

  @Override
  public void add(Component component) {
    componentList.add(component);
  }

  @Override
  public void remove(Component component) {
    componentList.remove(component);
  }

  @Override
  public void print() {
    System.out.println("====" + name + "========");
    for (Component element : componentList) {
      element.print();
    }
  }
}
{% endhighlight %}

College的程式碼與University一模一樣
{% highlight java linenos %}
public class College extends Component{
  List<Component> componentList = new ArrayList<>();

  public College(String name) {
    super();
    this.name = name;
  }

  @Override
  public void add(Component component) {
    componentList.add(component);
  }

  @Override
  public void remove(Component component) {
    componentList.remove(component);
  }

  @Override
  public void print() {
    System.out.println("====" + name + "========");
    for (Component element : componentList) {
      element.print();
    }
  }
}
{% endhighlight %}

Department，葉子節點
{% highlight java linenos %}
public class Department extends Component {
  public Department(String name) {
    super();
    this.name = name;
  }

  @Override
  public void print() {
    System.out.println(name);
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Component university = new University("XX大學");

    Component eletricCollege = new College("電資學院");
    Department edpt1 = new Department("通訊系");
    Department edpt2 = new Department("資工系");
    Department edpt3 = new Department("電機系");
    eletricCollege.add(edpt1);
    eletricCollege.add(edpt2);
    eletricCollege.add(edpt3);

    Component businessCollege = new College("商學院");
    Department bdpt1 = new Department("通訊系");
    Department bdpt2 = new Department("資工系");
    Department bdpt3 = new Department("電機系");
    businessCollege.add(bdpt1);
    businessCollege.add(bdpt2);
    businessCollege.add(bdpt3);

    university.add(eletricCollege);
    university.add(businessCollege);
    university.print();
  }
}
{% endhighlight %}
```
====XX大學========
====電資學院========
通訊系
資工系
電機系
====商學院========
通訊系
資工系
電機系
```