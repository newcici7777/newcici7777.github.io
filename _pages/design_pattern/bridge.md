---
title: 橋樑模式
date: 2025-05-03
keywords: Java, Design patterns, Bridge Pattern
---
Prerequisites:

- [HAS-A比IS-A更好][1]
- [依賴反轉原則][2]
- [開放關閉原則][3]
- [類別圖][4]

## 需求
為手機品牌A寫遊戲
{% highlight java linenos %}
class BrandAGame {
  public void play() {
    System.out.println("Brand A Game!");
  }
}
{% endhighlight %}

為手機品牌B寫遊戲
{% highlight java linenos %}
class BrandBGame {
  public void play() {
    System.out.println("Brand B Game!");
  }
}
{% endhighlight %}

把以上二個類別整合成如下

![img]({{site.imgurl}}/pattern/bridge1.png)

{% highlight java linenos %}
abstract class Game {
  abstract void play();
}

class BrandAGame extends Game {
  public void play() {
    System.out.println("Brand A Game!");
  }
}

class BrandBGame extends Game {
  public void play() {
    System.out.println("Brand B Game!");
  }
}
{% endhighlight %}

那如果要再增加通訊錄在各品牌手機，要如何修正？如下圖嗎？

![img]({{site.imgurl}}/pattern/bridge2.png)

那如果要再增加新的手機品牌，與拍照功能要如何修正？如下圖嗎？

是依軟體來分嗎？

![img]({{site.imgurl}}/pattern/bridge3.png)

還是依品牌來分？

![img]({{site.imgurl}}/pattern/bridge4.png)

若還要增加10種手機品牌，還要新增錄音、導航、行事曆…等等軟體，請問類別圖怎麼畫？

## 有一個比是一個更好

這種已經無法畫下去了，首先我們需要取消繼承關係，不要軟體中有品牌，也不要品牌中有軟體，要符合「單一職責原則」，一個類別就做一件事，分成「手機品牌抽象類別」與「軟體抽象類別」二類。

而在手機品牌類別中，用[有一個Has-A][1]的方式，把「軟體抽象類別」聚合在手機品牌中，使用抽象類別或介面(Interface)符合[依賴反轉原則][2]，依賴抽象，而不是依賴具體的子類別。

而且新增手機品牌與新增錄音、導航、行事曆…等等軟體，不用改動架構，比較好加入新的類別，符合[開放關閉原則][3]，對修改關閉，對擴充開放。

![img]({{site.imgurl}}/pattern/bridge5.png)

## 橋樑
注意上圖，那條菱形的線，橫跨手機品牌與軟體，就像一個橋，把二個不同的類別連接在一起，所以才稱之為橋樑模式(Bridge Pattern)。

## 經典橋樑模式類別圖
![img]({{site.imgurl}}/pattern/bridge6.png)

Abstraction意思是抽象，「手機品牌」，就是一個抽象的概念，手機品牌A、手機品牌B是品牌的子類別。

Implementor意思是實作，「軟體」有很多不同的軟體與實作方式。

但我覺得不用為名詞特別綁定一定要是抽象的概念才能歸類在Abstraction，我一開始也為此卡住，此模式的精神在於分類，單一職責、有一個比是一個更好、依賴反轉原則、開放關閉原則。

## 程式碼
![img]({{site.imgurl}}/pattern/bridge7.png)

軟體
{% highlight java linenos %}
public interface Software {
  void operationImp();
}

public class Game implements Software{
  @Override
  public void operationImp() {
    System.out.println("遊戲開始");
  }
}

public class Contacts implements Software{
  @Override
  public void operationImp() {
    System.out.println("列出聯絡人");
  }
}
{% endhighlight %}


品牌

最重要在於software成員變數是set過來的，在operation()方法是呼叫software.operationImp()。
{% highlight java linenos %}
public abstract class Brand {
  private Software software;

  public void operation() {
    software.operationImp();
  }

  public void setSoftware(Software software) {
    this.software = software;
  }
}

public class BrandA extends Brand{
  @Override
  public void operation() {
    super.operation();
    System.out.println("Brand A");
  }
}

public class BrandB extends Brand{
  @Override
  public void operation() {
    super.operation();
    System.out.println("BrandB");
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println("======Brand A=========");
    Brand brandA = new BrandA();
    brandA.setSoftware(new Game());
    brandA.operation();
    brandA.setSoftware(new Contacts());
    brandA.operation();
    System.out.println("======Brand B=========");
    Brand brandB = new BrandB();
    brandB.setSoftware(new Game());
    brandB.operation();
    brandB.setSoftware(new Contacts());
    brandB.operation();
  }
}
{% endhighlight %}
```
======Brand A=========
遊戲開始
Brand A
列出聯絡人
Brand A
======Brand B=========
遊戲開始
BrandB
列出聯絡人
BrandB
```

[1]: {% link _pages/design_pattern/has_a.md %}
[2]: {% link _pages/design_pattern/depend_inverse.md %}
[3]: {% link _pages/design_pattern/open_close.md %}
[4]: {% link _pages/java/uml.md %} 