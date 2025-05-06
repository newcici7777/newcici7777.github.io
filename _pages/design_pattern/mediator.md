---
title: 中介者模式
date: 2025-05-06
keywords: Java, Design patterns, Mediator pattern
---
## 中介者模式
Mediator，中文是調解員，在「深入淺出設計模式」一書是協調者。  
意思是，類別與類別不互相溝通，由調解員來做中間者，幫助類別彼此溝通，就像買屋的人與賣屋的人不互相溝通，由房仲跟二邊的人溝通。

## 類別圖
![img]({{site.imgurl}}/pattern/mediator1.png)

- AbstructColleague: 抽象同事類別
- Colleague: 具體同事類別
- Manager: 主管類別
- Mediator 抽象中介者
- Union 工會

AbstructColleague與Mediator是一條直線，因為二邊都有彼此的成員變數，是1對1雙向關係。

有一個說明，Mediator.sendMessage()，第2個參數會用到AbstractColleague物件，並callback回調AbstractColleague.receiveMessage()

## Mediator抽象中介者

### 要讓中間人，知道是誰要彼此溝通。
Mediator類別有個屬性colleague"s"，類型是List。

addColleague()方法，放置要透過中間人溝通的類別。

### callback回調
Mediator類別sendMessage(String message, AbstractColleague sender)

這個方法會呼叫AbstractColleague.receiveMessage()的方法。

### 程式碼
{% highlight java linenos %}
public abstract class Mediator {
  // 要透過中間人幫助溝通的類別
  private List<AbstractColleague> colleagues;

  public Mediator() {
    colleagues = new ArrayList<>();
  }

  // 加入要溝通的類別
  public void addColleague(AbstractColleague colleague) {
    colleagues.add(colleague);
  }

  // 處理中間人怎麼跟類別溝通
  public abstract void sendMessage(String message, AbstractColleague sender);

  public List<AbstractColleague> getColleagues() {
    return colleagues;
  }
}
{% endhighlight %}

## Union工會
作為中間人，與其它類別溝通。
{% highlight java linenos %}
// 工會，中間人
public class Union extends Mediator {
  // 參數2是發訊息的人
  @Override
  public void sendMessage(String message, AbstractColleague sender) {
    // 發送訊息給所有人
    for (AbstractColleague user: getColleagues()) {
      // 除了發送訊息本人之外，把訊息發給所有人
      if (!user.name.equals(sender.getName())) {
        // Callback回調
        user.receiveMessage(sender.getName() + "說" + message);
      }
    }
  }
}
{% endhighlight %}

## AbstractColleague抽象同事
- 要把中間人 Mediator 作為成員屬性
- 抽象的sendMessage()，子類別要覆寫
- 抽象的receiveMessage()，子類別要覆寫

AbstractColleague
{% highlight java linenos %}
public abstract class AbstractColleague {
  // Mediator 成員屬性
  protected Mediator mediator;
  // 姓名
  protected String name;

  // 建構子
  public AbstractColleague(Mediator mediator, String name) {
    this.mediator = mediator;
    this.name = name;
  }

  // 發送訊息，實際上是呼叫Mediator.sendMessage()
  public abstract void sendMessage(String message);
  
  // CallBack回調方法
  public abstract void receiveMessage(String message);

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }
}
{% endhighlight %}

## Colleague同事
設定回調Callback物件，在sendMessage()方法，實際上是呼叫Mediator.sendMessage()，第二個參數this，是把自己作為Callback回調物件。

Colleague
{% highlight java linenos %}
public class Colleague extends AbstractColleague{
  public Colleague(Mediator mediator, String name) {
    super(mediator, name);
  }

  // 實際上是呼叫Mediator.sendMessage()
  // 第2個參數是設定回調Callback物件
  public void sendMessage(String message) {
    mediator.sendMessage(message, this);
  }

  // CallBack回調方法
  public void receiveMessage(String message) {
    System.out.println("name = " + getName() + ", msg = " + message);
  }
}
{% endhighlight %}

## Manager主管
{% highlight java linenos %}
public class Manager extends AbstractColleague{
  public Manager(Mediator mediator, String name) {
    super(mediator, name);
  }

  public void sendMessage(String message) {
    mediator.sendMessage(message, this);
  }

  public void receiveMessage(String message) {
    System.out.println("Manager name = " + getName() + ", msg = " + message);
  }
}
{% endhighlight %}

## Client測試
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) {
    Union union = new Union();  //工會

    // 同事
    AbstractColleague colleague1 = new Colleague(union, "Alex");
    AbstractColleague colleague2 = new Colleague(union, "May");
    AbstractColleague colleague3 = new Colleague(union, "Bill");
    AbstractColleague manager1 = new Manager(union, "主管1");
    AbstractColleague manager2 = new Manager(union, "主管2");

    // 要讓中介者知道要協助溝通的人 
    union.addColleague(colleague1);
    union.addColleague(colleague2);
    union.addColleague(colleague3);
    union.addColleague(manager1);
    union.addColleague(manager2);

    // 向中介者發送訊息
    colleague1.sendMessage("Hello!");
    colleague2.sendMessage("Hi");
    manager2.sendMessage("I'm manager.");
  }
}
{% endhighlight %}
```
name = May, msg = Alex說Hello!
name = Bill, msg = Alex說Hello!
Manager name = 主管1, msg = Alex說Hello!
Manager name = 主管2, msg = Alex說Hello!
name = Alex, msg = May說Hi
name = Bill, msg = May說Hi
Manager name = 主管1, msg = May說Hi
Manager name = 主管2, msg = May說Hi
name = Alex, msg = 主管2說I'm manager.
name = May, msg = 主管2說I'm manager.
name = Bill, msg = 主管2說I'm manager.
Manager name = 主管1, msg = 主管2說I'm manager.
```
