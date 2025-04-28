---
title: Simple Factory
date: 2025-04-25
keywords: Java, Design patterns, Simple Factory
---
Prerequisites:

- [開放關閉原則][1]

## 未優化前
以下是一個訂購Pizza與制作Pizza的流程。

### 制作Pizza
Pizza
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
{% endhighlight %}

CheesePizza
{% highlight java linenos %}
public class CheesePizza extends Pizza{
  @Override
  public void prepare() {
    System.out.println(getName() + "有:起士");
  }
}
{% endhighlight %}

TomatoPizza
{% highlight java linenos %}
public class TomatoPizza extends Pizza{
  @Override
  public void prepare() {
    System.out.println(getName() + "有:蕃茄");
  }
}
{% endhighlight %}

### 訂購pizza
OrderPizza
{% highlight java linenos %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
public class OrderPizza {
  public OrderPizza() {
    Pizza pizza = null;
    String pizzaType;
    do {
      pizzaType = getType();
      if (pizzaType.equals("tomato")) {
        pizza = new TomatoPizza();
        pizza.setName("瑪格麗特pizza");
      } else if (pizzaType.equals("cheese")) {
        pizza = new CheesePizza();
        pizza.setName("起士pizza");
      } else {
        // 輸入其它的字就離開迴圈
        break;
      }
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

main 主程式
{% highlight java linenos %}
public class PizzaStore {
  public static void main(String[] args) {
    OrderPizza orderPizza = new OrderPizza();
  }
}
{% endhighlight %}
```
input pizza type:
cheese
起士pizza有:起士
放進烤箱
把pizza切片
pizza裝進盒子
input pizza type:
tomato
瑪格麗特pizza有:蕃茄
放進烤箱
把pizza切片
pizza裝進盒子
input pizza type:
a
```

## 優化
以上的問題是，為什麼產生pizza的程式碼在訂購的流程中？
{% highlight java linenos %}
  if (pizzaType.equals("tomato")) {
    pizza = new TomatoPizza();
    pizza.setName("瑪格麗特pizza");
  } else if (pizzaType.equals("cheese")) {
    pizza = new CheesePizza();
    pizza.setName("起士pizza");
  } else {
    // 輸入其它的字就離開迴圈
    break;
  }
{% endhighlight %}

把產生pizza的程式碼放入工廠的類別中，由Pizza工廠來生產pizza，只需要給它pizzaType的參數。

SimpleFactory
{% highlight java linenos %}
public class SimpleFactory {
  public Pizza createPizza(String pizzaType) {
    Pizza pizza = null;
    if (pizzaType.equals("tomato")) {
      pizza = new TomatoPizza();
      pizza.setName("瑪格麗特pizza");
    } else if (pizzaType.equals("cheese")) {
      pizza = new CheesePizza();
      pizza.setName("起士pizza");
    }
    return pizza;
  }
}
{% endhighlight %}

OrderPizza類別增加設置Pizza工廠
{% highlight java linenos %}
  private SimpleFactory factory;
  public void setFactory(SimpleFactory factory) {
    this.factory = factory;
  }
{% endhighlight %}

OrderPizza增加order()的方法，只負責取得pizza的種類、事前準備prepare()、烤、切、裝盒子。
{% highlight java linenos %}
public void order() {
  Pizza pizza = null;
  String pizzaType;
  do {
    pizzaType = getType();
    // 產生pizza
    pizza = factory.createPizza(pizzaType);
    // 跳離迴圈
    if (pizza == null) break;
    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();
  } while (true);
}
{% endhighlight %}

類別圖如下:

setFactory是聚合的關係，所以是用空白菱型，請見[類別圖][2]文章。

![img]({{site.imgurl}}/pattern/simple_factory1.png)

完整的OrderPizza
{% highlight java linenos %}
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
public class OrderPizza {
  private SimpleFactory factory;
  public void setFactory(SimpleFactory factory) {
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
  public OrderPizza() {
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

[1]: {% link _pages/c/design_pattern/open_close.md %}
[2]: {% link _pages/java/uml.md %}