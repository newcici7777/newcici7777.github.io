---
title: 建造者模式
date: 2025-04-30
keywords: Java, Design patterns, Builder Pattern
---
建造者模式，把「產品」與「建造過程」與「控制那個流程誰先誰後」，三者分開，三者解耦，符合[迪米特原則][1]。

## 經典的建造者類別圖
![img]({{site.imgurl}}/pattern/builder1.png)

### Product產品
要做出來的東西

### Builder建造者介面
定義要做出東西的「過程」或「步驟」。

以下為要做出東西的方法method:

buildPartA()、buildPartB()、buildPartC()

這些過程是抽象方法，由實作的子類別去覆寫這些方法。

成員屬性有product，用組合(黑色菱形)的方法，用new Product()方式組合在Builder介面。

### ConcreateBuilder實作建造者
覆寫抽象方法buildPartA()、buildPartB()、buildPartC()

在這些方法寫上製作這個產品的實際內容。

### Director指揮者
成員屬性有builder，用聚合(空心菱形)的方法，用setBuilder()方式聚合。

construct()方法，把製作產品的過程、步驟、方法統整起來，最後傳回一個Product產品出來。

## 建造各種房子
以上是經典類別圖，現在根據經典類別圖，來建造房子吧！我們要建造小木屋Cabin、別墅Villa二種房子，建造過程一定是不同的。

![img]({{site.imgurl}}/pattern/builder2.png)

### Builder建造者介面
以下為建立房子的過程:

buildBase()建造地基

buildWalls()建造牆壁

buildRoof()建造屋頂

build()會傳回house物件

### ConcreateBuilder實作建造者
CabinBuilder、VillaBuilder有二個建造者。

## 程式碼
House，這次要建造的產品是房子。
{% highlight java linenos %}
public class House {
  private String base;
  private String roof;
  private String wall;

  public String getBase() {
    return base;
  }

  public void setBase(String base) {
    this.base = base;
  }

  public String getRoof() {
    return roof;
  }

  public void setRoof(String roof) {
    this.roof = roof;
  }

  public String getWall() {
    return wall;
  }

  public void setWall(String wall) {
    this.wall = wall;
  }
}
{% endhighlight %}

Builder建造者抽象類別
{% highlight java linenos %}
public abstract class Builder {
  // 用組合的方式，把house組合在Builder介面
  protected House house = new House();
  
  // 建地基
  public abstract void buildBase();
  
  // 建牆壁
  public abstract void buildWalls();
  
  // 建屋頂
  public abstract void buildRoof();
  
  // 傳回house物件
  public House build() {
    return house;
  }
}
{% endhighlight %}

CabinBuilder小木屋建造者
{% highlight java linenos %}
public class CabinBuilder extends Builder{
  @Override
  public void buildBase() {
    house.setBase("木頭地基");
    System.out.println("開始建造木頭地基");
  }

  @Override
  public void buildWalls() {
    house.setWall("木牆");
    System.out.println("開始建造木牆");
  }

  @Override
  public void buildRoof() {
    house.setRoof("木製屋頂");
    System.out.println("開始建造木製屋頂");
  }
}
{% endhighlight %}

VillaBuilder別墅建造者
{% highlight java linenos %}
public class VillaBuilder extends Builder{
  @Override
  public void buildBase() {
    house.setBase("鋼筋水泥地基");
    System.out.println("開始建造鋼筋水泥地基");
  }

  @Override
  public void buildWalls() {
    house.setWall("鋼筋水泥牆");
    System.out.println("開始建造鋼筋水泥牆");
  }

  @Override
  public void buildRoof() {
    house.setRoof("鋼筋水泥屋頂 + 游泳池");
    System.out.println("開始建造防漏水鋼筋水泥屋頂 + 游泳池");
  }
}
{% endhighlight %}

Director指揮者，負責建造流程與產生房子。
{% highlight java linenos %}
public class Director {
  private Builder builder;
  
  // 用聚合的方式，把builder聚合在Director
  public void setBuilder(Builder builder) {
    this.builder = builder;
  }

  // 控管建造流程，最後把建造完成的房子傳回
  public House construct() {
    builder.buildBase();
    builder.buildWalls();
    builder.buildRoof();
    return builder.build();
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 指揮者
    Director director = new Director();
    System.out.println("==========建造小木屋===========");
    // 把小木屋建造者放入參數
    director.setBuilder(new CabinBuilder());
    // 建造小木屋
    House house = director.construct();
    System.out.println("Base = " + house.getBase());
    System.out.println("Wall = " + house.getWall());
    System.out.println("Roof = " + house.getRoof());

    System.out.println("========建造別墅=============");
    // 把別墅建造者放入參數
    director.setBuilder(new VillaBuilder());
    // 建造別墅
    house = director.construct();
    System.out.println("Base = " + house.getBase());
    System.out.println("Wall = " + house.getWall());
    System.out.println("Roof = " + house.getRoof());
  }
}
{% endhighlight %}
```
==========建造小木屋===========
開始建造木頭地基
開始建造木牆
開始建造木製屋頂
Base = 木頭地基
Wall = 木牆
Roof = 木製屋頂
========建造別墅=============
開始建造鋼筋水泥地基
開始建造鋼筋水泥牆
開始建造防漏水鋼筋水泥屋頂 + 游泳池
Base = 鋼筋水泥地基
Wall = 鋼筋水泥牆
Roof = 鋼筋水泥屋頂 + 游泳池
```

[1]: {% link _pages/design_pattern/demeter.md %}