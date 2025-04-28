---
title: 抽象工廠
date: 2025-04-25
keywords: Java, Design patterns, Abstract Factory
---
Prerequisites:

- [簡單工廠模式][1]

在簡單工廠模式中，使用工廠生產各個種類的Pizza。

若今天在各個國家都有工廠，根據不同國家生產符合他們口味的Pizza，請問要怎麼改？

假設現在有台灣Pizza工廠、日本Pizza工廠

台灣工廠的口味是珍珠奶茶Pizza、臭豆腐Pizza

日本工廠的口味是章魚燒Pizza、大阪燒Pizza

首先要先建立的抽象工廠，裡面有一個createPizza()方法，由各個工廠去覆寫。

![img]({{site.imgurl}}/pattern/absfactory1.png)

Pizza抽象類別，下面有各種口味的Pizza。

還有一個訂購Pizza的類別

如下圖:

![img]({{site.imgurl}}/pattern/absfactory2.png)

工廠相關程式碼
{% highlight java linenos %}
public abstract class AbsFactory {
  public abstract Pizza createPizza(String pizzaType);
}

public class TWFactory extends AbsFactory{
  @Override
  public Pizza createPizza(String pizzaType) {
    Pizza pizza = null;
    if (pizzaType.equals("tofu")) {
      pizza = new TofuPizza();
      pizza.setName("臭豆腐pizza");
    } else if (pizzaType.equals("bubble")) {
      pizza = new BubblePizza();
      pizza.setName("珍珠奶茶pizza");
    }
    return pizza;
  }
}

public class JAPFactory extends AbsFactory{
  @Override
  public Pizza createPizza(String pizzaType) {
    Pizza pizza = null;
    if (pizzaType.equals("taco")) {
      pizza = new TacoPizza();
      pizza.setName("章魚燒pizza");
    } else if (pizzaType.equals("oko")) {
      pizza = new BubblePizza();
      pizza.setName("大阪燒pizza");
    }
    return pizza;
  }
}
{% endhighlight %}

Pizza相關程式碼
{% highlight java linenos %}
public abstract class Pizza {
  private String name;
  public void setName(String name) {
    this.name = name;
  }
  public String getName() {
    return name;
  }
  // 不同的pizza準備的材料不同，由不同的pizza去覆寫
  public abstract void prepare();
  // 相同的作法就統一寫
  public void bake() {
    System.out.println("放進烤箱");
  }
  public void cut() {
    System.out.println("把pizza切片");
  }
  public void box() {
    System.out.println("pizza裝進盒子");
  }
}

public class BubblePizza extends Pizza{
  @Override
  public void prepare() {
    System.out.println(getName() + "有:珍珠");
  }
}

public class TofuPizza extends Pizza{
  @Override
  public void prepare() {
    System.out.println(getName() + "有:臭豆腐");
  }
}

public class TacoPizza extends Pizza{
  @Override
  public void prepare() {
    System.out.println(getName() + "有:章魚燒");
  }
}

public class OkoPizza extends Pizza{
  @Override
  public void prepare() {
    System.out.println(getName() + "有:高麗菜");
  }
}
{% endhighlight %}

訂購相關
{% highlight java linenos %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
public class OrderPizza {
  private AbsFactory factory;
  public void setFactory(AbsFactory factory) {
    this.factory = factory;
  }
  public void order() {
    Pizza pizza = null;
    String pizzaType;
    do {
      pizzaType = getType();
      pizza = factory.createPizza(pizzaType);
      if (pizza == null) break;
      pizza.prepare();
      pizza.bake();
      pizza.cut();
      pizza.box();
    } while (true);
  }
  private String getType() {
    try {
      BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));
      System.out.println("input pizza type:");
      String str = strin.readLine();
      return str;
    } catch (IOException e) {
      e.printStackTrace();
      return "";
    }
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    OrderPizza orderPizza = new OrderPizza();
    orderPizza.setFactory(new JAPFactory());
    orderPizza.order();
  }
}
{% endhighlight %}
```
input pizza type:
oko
大阪燒pizza有:珍珠
放進烤箱
把pizza切片
pizza裝進盒子
input pizza type:
taco
章魚燒pizza有:章魚燒
放進烤箱
把pizza切片
pizza裝進盒子
```

[1]: {% link _pages/design_pattern/simple_factory.md %}
