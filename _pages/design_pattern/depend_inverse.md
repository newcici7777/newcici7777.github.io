---
title: 依賴反轉原則
date: 2025-04-22
keywords: Java, Design patterns, Dependence Inversion Principle
---
Prerequisites:

- [類別圖][1]
- [多型][2]
- [介面][3]
- [抽象][4]

## 概念
英文Dependence Inversion Principle，

> Abstractions should not depend on details. Details should depend on abstractions.

依賴抽象，不要依賴具體類別。

這裡的抽象包括介面，因為介面中都是抽象方法。

也跟之前提到「寫程式是針對抽象、介面而寫，而不是針對實踐方式(具體類別)而寫」，是一樣的意思。

## 未優化前程式碼
以下程式碼，Person收到e-mail訊息並把訊息顯示出來。

{% highlight java linenos %}
class Email {
  void getInfo() {
    System.out.println("e-mail");
  }
}

class Person {
  void receive(Email email) {
    email.getInfo();
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Person person = new Person();
    person.receive(new Email());
  }
}
{% endhighlight %}

## 優化思維

請問，若今天要收到Line、收到手機簡訊、收到FB的Message請問要怎麼做？是改成如下的方式嗎？那如果有100個通訊軟體，要一直寫100個receive()方法？100個參數？

{% highlight java linenos %}
class Person {
  void receive(Email email) {
    email.getInfo();
  }
  void receive(Line line) {
    line.getInfo();
  }
  void receive(SMS sms) {
    sms.getInfo();
  }
  void receive(FBMessage fbMessage) {
    fbMessage.getInfo();
  }
}
{% endhighlight %}

寫程式是針對抽象、介面而寫，而不是針對具體類別而寫。

所以我們要做一個抽象類別或介面，裡面定義抽象方法，由實作介面的類別或繼承抽象類別的子類別，去實作抽象方法。

下圖中，介面IReceive有一個抽象方法getInfo()，實作此介面的類別(Email, Line, SMS, FBMessage)都要覆寫(Override)getInfo()的方法。

![img]({{site.imgurl}}/pattern/ireceive.png)

下圖中，Person類別的receive()方法使用到IReceive這個介面，作為方法參數。

![img]({{site.imgurl}}/pattern/ireceive2.png)

程式碼如下:

{% highlight java linenos %}
interface IReceive {
  void getInfo();
}

class Email implements IReceive{
  @Override
  public void getInfo() {
    System.out.println("email");
  }
}

class FBMessage implements IReceive{
  @Override
  public void getInfo() {
    System.out.println("FB Message");
  }
}

class Line implements IReceive{
  @Override
  public void getInfo() {
    System.out.println("Line");
  }
}
{% endhighlight %}

Person
{% highlight java linenos %}
class Person {
  void receive(IReceive receive) {
    receive.getInfo();
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
  public static void main(String[] args) {
    Person person = new Person();
    person.receive(new Email());
    person.receive(new Line());
    person.receive(new FBMessage());
  }
{% endhighlight %}
```
email
Line
FB Message
```

[1]: {% link _pages/java/uml.md %}
[2]: {% link _pages/java/polymorphism.md %}
[3]: {% link _pages/java/interface.md %}
[4]: {% link _pages/java/abstract.md %}
