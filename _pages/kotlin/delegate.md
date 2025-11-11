---
title: 委托 by Delegation
date: 2025-11-11
keywords: kotlin, by, Delegation
---
## 類別委托
Kotlin的委托，我覺得十分像設計模式中的[代理人模式][1]。

就是旅客跟旅行社買機票，但實際付錢是旅客付。

{% highlight kotlin linenos %}
// BuyTicket介面 要做的事
interface BuyTicket {
    fun pay()
}

// 旅客實作BuyTicket介面的pay方法。
// 真正要做事的人
class Tourist : BuyTicket {
    override fun pay() = println("旅客提供信用卡 卡號付錢")
}

// 旅行社實作BuyTicket介面的pay方法，實際上是旅客付錢。
// 代理人
class Agency(tourist: BuyTicket) : BuyTicket by tourist
{% endhighlight %}

{% highlight kotlin linenos %}
fun main() {
    // 真正要做事的人
    val tourist = Tourist()
    // 代理人，把 真正要做事的人 的傳入
    val agency = Agency(tourist)
    // 實際上是旅客付錢，不是代理人付錢
    agency.pay()
}
{% endhighlight %}
```
旅客提供信用卡 卡號付錢
```

以上的內容要有Java程式碼對映才比較明白。<br>

BuyTicket介面
{% highlight java linenos %}
public interface BuyTicket {
  void pay();
}
{% endhighlight %}

旅客實作BuyTicket介面的pay方法。
{% highlight java linenos %}
public class Tourist implements BuyTicket{
  @Override
  public void pay() {
    System.out.println("旅客提供信用卡 卡號付錢");
  }
}
{% endhighlight %}

旅行社實作BuyTicket介面的pay方法，實際上是旅客付錢。
{% highlight java linenos %}
public class Agency implements BuyTicket{
  // 成員屬性要有旅客Tourist
  private Tourist tourist;

  // 使用建構子參數，把旅客傳入旅行社
  public Agency(Tourist tourist) {
    this.tourist = tourist;
  }

  @Override
  public void pay() {
    // 實際上是呼叫旅客的pay()方法
    tourist.pay();
  }
}
{% endhighlight %}

Client測試
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) {
    // 建立旅客
    Tourist tourist = new Tourist();
    // 把旅客傳入旅行社的建構子
    Agency agency = new Agency(tourist);
    // 旅行社付錢，實際上旅客付的。
    agency.pay();
  }
}
{% endhighlight %}
```
旅客提供信用卡 卡號付錢
```

語法
```
class 代理人(真正要做事的人: 要做的事) : 要做的事 by 真正要做事的人
class Agency(tourist: BuyTicket) : BuyTicket by tourist
```


[1]: {% link _pages/design_pattern/proxy.md %}
