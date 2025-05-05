---
title: 狀態模式
date: 2025-05-05
keywords: Java, Design patterns, State Pattern
---
這是一個轉變狀態的模式。

## 需求
設計一個線上購物程式，並設計以下幾種狀態。

- 無訂單狀態
- 訂購商品
- 裝箱
- 寄送商品
- 接收產品

訂購產品的時候，由無訂單狀態 -> 訂購商品狀態 -> 裝箱狀態 -> 寄送狀態 -> 接收產品狀態 -> 無訂單狀態

## 類別圖
![img]({{site.imgurl}}/pattern/state1.png)

### Eshopping
儲存共用的變數count，各種狀態(沒訂單狀態、訂單狀態、裝箱狀態、寄送狀態、收到狀態)、state為目前狀態。

- setState()設定目前狀態
- order() 呼叫orderState訂單狀態的order()
- box() 呼叫boxState裝箱狀態的box()
- delivery() 呼叫deliveryState寄送狀態的delivery()
- receiveState() 呼叫receiveState接收狀態的receive()
- noOrder() 呼叫noOrderState沒有訂單狀態的noOrder()
- setCount() 設定產品變數

{% highlight java linenos %}
public class Eshopping {
  // 沒訂單狀態
  private State noOrderState;
  // 訂單狀態
  private State orderState;
  // 裝箱狀態
  private State boxState;
  // 寄送狀態
  private State deliveryState;
  // 收到狀態
  private State receiveState;
  // 目前狀態
  private State state;
  // 產品數量
  private int count = 0;

  public Eshopping() {
    // 建立所有狀態
    noOrderState = new NoOrderState(this);
    orderState = new OrderState(this);
    boxState = new BoxState(this);
    deliveryState = new DeliveryState(this);
    receiveState = new ReceiveState(this);
    // 預設為沒有訂單狀態
    state = noOrderState;
  }
  
  // 沒有訂單
  public void noOrder() {
    state.noOrder();
  }
  
  // 訂購
  public void order() {
    state.order();
  }

  // 產品裝箱
  public void box() {
    state.box();
  }
  
  // 產品寄送
  public void delivery() {
    state.deliver();
  }
  
  // 收到產品
  public void receive() {
    state.receive();
  }
  
  // 設定狀態
  public void setState(State state) {
    this.state = state;
  }
  
  // 取得無訂單狀態
  public State getNoOrderState() {
    return noOrderState;
  }

  public State getOrderState() {
    return orderState;
  }

  public State getBoxState() {
    return boxState;
  }

  public State getDeliveryState() {
    return deliveryState;
  }

  public State getReceiveState() {
    return receiveState;
  }
  
  // 設定產品數量
  public void setCount(int count) {
    this.count = count;
  }
  
  //取得產品數量
  public int getCount() {
    return count;
  }
}
{% endhighlight %}

## State介面
定義介面要實作的方法
{% highlight java linenos %}
public interface State {
  // 沒訂單
  void noOrder();
  // 訂購
  void order();
  // 裝箱
  void box();
  // 寄送
  void deliver();
  // 收到產品
  void receive();
}
{% endhighlight %}

## AbstractState抽象狀態
此處用到[介面轉接器模式][1]

把介面所有的方法全部實作，繼承的子類別，只要選擇性覆寫特定方法。

{% highlight java linenos %}
public abstract class AbstractState implements State {
  @Override
  public void noOrder() {
    // do nothing
  }

  @Override
  public void order() {
    // do nothing
  }

  @Override
  public void box() {
    // do nothing
  }

  @Override
  public void deliver() {
    // do nothing
  }

  @Override
  public void receive() {
    // do nothing
  }
}
{% endhighlight %}

## 無訂單狀態
重點在於轉換到其它狀態的部分，狀態轉到訂單狀態，並呼叫訂單狀態的order方法。
{% highlight java linenos %}
public class NoOrderState extends AbstractState {
  private Eshopping eshopping;

  public NoOrderState(Eshopping eshopping) {
    this.eshopping = eshopping;
  }

  @Override
  public void noOrder() {
    if (eshopping.getCount() == 0)
      System.out.println("產品數量不足");
    System.out.println("無訂單狀態");
  }

  @Override
  public void order() {
    // 狀態轉到訂單狀態
    eshopping.setState(eshopping.getOrderState());
    // 呼叫訂購
    eshopping.order();
  }
}
{% endhighlight %}

## 訂購狀態
重點在於轉換到其它狀態的部分，訂單狀態轉到裝箱狀態，並呼叫裝箱狀態的box()。

若產品數量不足，無法提供訂購，訂單狀態轉到沒有訂單，並呼叫沒有訂單狀態的noOrder()。
{% highlight java linenos %}
public class OrderState extends AbstractState {
  private Eshopping eshopping;

  public OrderState(Eshopping eshopping) {
    this.eshopping = eshopping;
  }

  @Override
  public void order() {
    if (eshopping.getCount() > 0) {
      System.out.println("訂購產品");
      // 數量減1
      eshopping.setCount(eshopping.getCount() - 1);
      // 狀態轉到裝箱
      eshopping.setState(eshopping.getBoxState());
      eshopping.box();
    } else {
      // 狀態轉到沒有訂單
      eshopping.setState(eshopping.getNoOrderState());
      // 呼叫沒有訂單的方法
      eshopping.noOrder();
    }
  }
}
{% endhighlight %}

## 裝箱狀態
重點在於轉換到其它狀態的部分，狀態轉到寄送狀態，並呼叫寄送狀態的delivery()。
{% highlight java linenos %}
public class BoxState extends AbstractState{
  private Eshopping eshopping;
  public BoxState(Eshopping eshopping) {
    this.eshopping = eshopping;
  }
  @Override
  public void box() {
    // 裝箱
    System.out.println("裝箱完成");
    // 狀態轉到寄送
    eshopping.setState(eshopping.getDeliveryState());
    eshopping.delivery();
  }
}
{% endhighlight %}

## 寄送狀態
重點在於轉換到其它狀態的部分，狀態轉到接收狀態，並呼叫接收狀態的receivey()。
{% highlight java linenos %}
public class DeliveryState extends AbstractState {
  private Eshopping eshopping;
  public DeliveryState(Eshopping eshopping) {
    this.eshopping = eshopping;
  }

  @Override
  public void deliver() {
    System.out.println("出貨");
    // 狀態轉到接收
    eshopping.setState(eshopping.getReceiveState());
    eshopping.receive();
  }
}
{% endhighlight %}

## 接收狀態
重點在於轉換到其它狀態的部分，狀態轉到沒有訂單狀態，並呼叫沒有訂單狀態的noOrder()。
{% highlight java linenos %}
public class ReceiveState extends AbstractState {
  private Eshopping eshopping;
  public ReceiveState(Eshopping eshopping) {
    this.eshopping = eshopping;
  }

  @Override
  public void receive() {
    System.out.println("貨品已送達");
    // 狀態轉到沒有訂單
    eshopping.setState(eshopping.getNoOrderState());
    eshopping.noOrder();
  }
}
{% endhighlight %}

## 測試
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Eshopping eshopping = new Eshopping();
    //設定數量
    eshopping.setCount(2);

    //訂購
    eshopping.order();
    System.out.println("===========================");
    eshopping.order();
    System.out.println("===========================");
    eshopping.order();
  }
}
{% endhighlight %}
```
訂購產品
裝箱完成
出貨
貨品已送達
商品剩餘數量 = 1
===========================
訂購產品
裝箱完成
出貨
貨品已送達
商品剩餘數量 = 0
===========================
產品數量不足
無訂單狀態
商品剩餘數量 = 0
```

[1]: {% link _pages/design_pattern/interface_adapter.md %}