---
title: 責任鏈
date: 2025-04-29
keywords: Java, Design patterns, Chain of Responsibility Pattern
---
## 需求
想像一下你是公務員，有很多承包商的報價需求，需要長官審核，長官有下列幾位

|職等|審核金額|
|主管|0 - 49999|
|會計|50000 - 99999|
|老闆|10000以上|

主管會處理0 - 49999元，金額超過這個範圍，給會計處理。

會計處理5萬以上，10萬以下，金額超過這個範圍，給老闆處理。

老闆只處理10萬元以上，10萬元以下，老闆不處理，給主管處理。

有看出這個環狀結果嗎，主管->會計->老闆->主管。

## 類別圖
![img]({{site.imgurl}}/pattern/chain1.png)

Request是報價需求，有編號與價錢二個成員屬性。

Approver是抽象類別，中文意思是簽呈者，成員屬性有nextApprover設定下一個處理的人，process()是抽象方法，繼承的子類別一定要覆寫。

Approver子類別如下ManagerApprover, AccountApprover, BossApprover，分別是主管簽呈、會計簽呈、老闆簽呈。

## Request需求
有編號與價錢二個成員屬性。
{% highlight java linenos %}
public class Request {
  private int id;
  private int price;

  public Request(int id, int price) {
    this.id = id;
    this.price = price;
  }

  public int getId() {
    return id;
  }

  public int getPrice() {
    return price;
  }
}
{% endhighlight %}

## Approver
{% highlight java linenos %}
public abstract class Approver {
  private String name;
  // nextApprover設定下一個處理的人
  protected Approver nextApprover;

  // 設定簽呈人的名字
  public Approver(String name) {
    this.name = name;
  }

  // 抽象方法，子類別一定要實作，處理需求
  public abstract void process(Request request);

  public String getName() {
    return name;
  }
  
  // 設定下一個處理的人
  public void setNextApprover(Approver nextApprover) {
    this.nextApprover = nextApprover;
  }
}
{% endhighlight %}

## Approver子類別
子類別都要覆寫process()抽象方法

主管
{% highlight java linenos %}
public class ManagerApprover extends Approver{
  public ManagerApprover(String name) {
    super(name);
  }

  @Override
  public void process(Request request) {
    // 處理金額範圍
    if (request.getPrice() > 0 && request.getPrice() <= 49999) {
      System.out.println("需求編號 =" + request.getId() +"被" + getName() + "處理");
    } else {
      nextApprover.process(request); // 給下一個人處理
    }
  }
}
{% endhighlight %}

會計
{% highlight java linenos %}
public class AccountApprover extends Approver{
  public AccountApprover(String name) {
    super(name);
  }

  @Override
  public void process(Request request) {
    // 處理金額範圍
    if (request.getPrice() >= 50000 && request.getPrice() <= 99999) {
      System.out.println("需求編號 =" + request.getId() +"被" + getName() + "處理");
    } else {
      nextApprover.process(request); // 給下一個人處理
    }
  }
}

{% endhighlight %}

老闆
{% highlight java linenos %}
public class BossApprover extends Approver{
  public BossApprover(String name) {
    super(name);
  }

  @Override
  public void process(Request request) {
    // 處理金額範圍
    if (request.getPrice() >= 100000) {
      System.out.println("需求編號 =" + request.getId() +"被" + getName() + "處理");
    } else {
      nextApprover.process(request); // 給下一個人處理
    }
  }
}
{% endhighlight %}

## main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 建立需求
    Request request1 = new Request(1, 200000);
    Request request2 = new Request(2, 3500);
    Request request3 = new Request(3, 50000);

    // 建立簽呈的人
    ManagerApprover managerApprover = new ManagerApprover("主管");
    AccountApprover accountApprover = new AccountApprover("會計");
    BossApprover bossApprover = new BossApprover("老闆");
    
    // 設定下一個處理的人
    managerApprover.setApprover(accountApprover);
    accountApprover.setApprover(bossApprover);
    // 設定若金額太小，下一個可以處理的人是誰，要形成環狀
    bossApprover.setApprover(managerApprover);

    // 處理需求
    managerApprover.process(request1);
    // 測試金額3500，會不會給下一個人處理
    bossApprover.process(request2);
    managerApprover.process(request3);
  }
}
{% endhighlight %}
```
需求編號 =1被老闆處理
需求編號 =2被主管處理
需求編號 =3被會計處理
```