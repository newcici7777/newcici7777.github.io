---
title: 裝飾者模式
date: 2025-04-29
keywords: Java, Design patterns, Decorator Pattern
---
## 需求
開了一家飲料店，有綠茶、紅茶…各種茶。

客人可以加錢加配料，比如加上珍珠、鮮奶、粉角、椰果、小芋圓，不同配料價錢不同。

珍珠可以加2份或3份、其它配料也是，沒有限定只能加一次。

設計一家飲料店，要求是不用修改程式，就可以添加新的茶飲與新的配料如仙草、米苔米，並計算添加配料後一杯飲料要多少錢。

以下複雜的計算流程是不被允許，若有新的配料又要改判斷與價錢。
{% highlight java linenos %}
int cost = 0;
if (有珍珠)
  cost += 5;
if (有粉角)
  cost += 10;
if (有鮮奶)
  cost += 15;
.
.
.
{% endhighlight %}

## 類別圖解釋
![img]({{site.imgurl}}/pattern/decorator1.png)

飲料(Drink)與配料(Decorator)都繼承抽象類別Component，所以飲料與配料也是Component，飲料與配料都要覆寫cost()方法。

注意！配料(Decorator)中有一個屬性是component，它可以是飲料Drink也可以是配料Decorator，所以才有二個黑色菱形都指向component屬性。

黑色菱形為組合，會使用建構子，參數為飲料或配料，建構子傳遞參數就是組合，請見[類別圖][1]。

飲料Drink下面有綠茶、紅茶、咖啡。

配料Decorator下面有牛奶、珍珠、果凍。

## Component抽象類別
{% highlight java linenos %}
public abstract class Component {
  // 價格
  private int price;
  // 描述
  private String desc;

  // 抽象方法
  abstract int cost();

  // 取得價格
  public int getPrice() {
    return price;
  }

  // 設定價格
  public void setPrice(int price) {
    this.price = price;
  }

  public String getDesc() {
    return desc;
  }

  public void setDesc(String desc) {
    this.desc = desc;
  }
}
{% endhighlight %}

## Drink飲料
{% highlight java linenos %}
public class Drink extends Component{
  @Override
  int cost() {
    // 使用父類別的getPrice()方法
    return super.getPrice();
  }
}
{% endhighlight %}

## Decorator配料
注意事項:
- component 成員屬性(十分重要)
- 建立建構子，參數就是component
- 覆寫cost()方法，使用super.getPrice() + component.cost()，注意！是加component.cost()，不是component.getPrice()
- 覆寫desc()方法，注意！使用super.getDesc()，以免stack overflow，不能自己呼叫自己。

{% highlight java linenos %}
public class Decorator extends Component{
  // component可能是飲料或配料
  private Component component;

  // component可能是飲料或配料
  public Decorator(Component component) {
    this.component = component;
  }

  @Override
  int cost() {
    // 使用父類別的getPrice()方法
    // 配料的價格 + component的「總費用」(Component可能是飲料或配料)
    return super.getPrice() + component.cost();
  }

  @Override
  public String getDesc() {
    // 使用父類別的getDesc()方法，使用this.getDesc()會stack overflow，因為自己呼叫自己
    // 印出配料的品項與配料的價格，componet的desc包含先前加上的配料品項與價格
    return super.getDesc() + "=" + super.getPrice() + " & " + component.getDesc();
  }
}
{% endhighlight %}

## 繼承Drink類別
- 設定價格
- 設定品項

紅茶
{% highlight java linenos %}
public class BlackTea extends Drink {
  public BlackTea() {
    // 設定價格
    setPrice(35);
    // 設定品項
    setDesc("紅茶 = " + getPrice());
  }
}
{% endhighlight %}

綠茶
{% highlight java linenos %}
public class GreenTea extends Drink{
  public GreenTea() {
    // 設定價格
    setPrice(40);
    // 設定品項
    setDesc("綠茶 = " + getPrice());
  }
}
{% endhighlight %}

## 繼承Decorator配料
- 設定價格
- 設定品項

珍珠
{% highlight java linenos %}
public class Bubble extends Decorator{
  public Bubble(Component component) {
    // 使用父類別Decorator建構子
    super(component);
    // 設定價格
    setPrice(5);
    // 設定品項
    setDesc("珍珠");
  }
}
{% endhighlight %}

牛奶
{% highlight java linenos %}
public class Milk extends Decorator {
  public Milk(Component component) {
    // 使用父類別Decorator建構子
    super(component);
    // 設定價格
    setPrice(10);
    // 設定品項
    setDesc("牛奶");
  }
}
{% endhighlight %}

## 使用方法
1. 先建立飲料
2. 用配料的建構子，傳進飲料

下圖中，包在最裡面的是飲料，其它都是配料。

![img]({{site.imgurl}}/pattern/decorator2.png)

可以想像一下，每一個方框都是建構子，包在最裡面飲料，其它都是配料。
{% highlight java linenos %}
new Jelly(new Bubble(new Milk(new BlackTea())));
{% endhighlight %}

所以先前才會說Decorator的建構子參數component可能是飲料或配料。

Main 主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 建立綠茶
    Component greenTea = new GreenTea();
    // 加牛奶
    greenTea = new Milk(greenTea);
    // 加第2份牛奶
    greenTea = new Milk(greenTea);
    // 加珍珠
    greenTea = new Bubble(greenTea);
    // 印出品項與價格
    System.out.println(greenTea.getDesc());
    // 印出全部花費
    System.out.println(greenTea.cost());
  }
}
{% endhighlight %}
```
珍珠=5 & 牛奶=10 & 牛奶=10 & 綠茶 = 40
65
```

[1]: {% link _pages/java/uml.md %}




