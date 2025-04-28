---
title: 物件轉接器
date: 2025-04-28
keywords: Java, Design patterns, Object Adapter Pattern
---
比如到大陸去旅遊，他們的電壓是220v，要買個轉接器把220v轉成5v，才能給手機充電使用，手機只能用5v。

下圖中轉接器Adapter用到Voltage220類別，Adapter要實作output5v的方法，把220v轉成5v輸出

因為使用new Adater()時把Voltage220類別傳入，所以是有生死相隨的關係，使用實心菱形，代表組合，組合的意思是關係無法分開，一起建立一起被記憶體回收。

Phone類別，有一個chage()充電的方法，用到Voltage5類別。

![img]({{site.imgurl}}/pattern/object_adapter1.png)

程式碼

Voltage5V
{% highlight java linenos %}
public interface Voltage5V {
  int output5v();
}
{% endhighlight %}

Voltage220
{% highlight java linenos %}
public class Voltage220 {
  int output220v() {
    return 220;
  }
}
{% endhighlight %}

Adapter
{% highlight java linenos %}
public class Adapter implements Voltage5V{
  private Voltage220 voltage220;
  public Adapter(Voltage220 voltage220) {
    this.voltage220 = voltage220;
  }
  @Override
  public int output5v() {
    return voltage220.output220v() / 44;
  }
}
{% endhighlight %}

Phone
{% highlight java linenos %}
public class Phone {
  public void charg(Voltage5V voltage5V) {
    if ( voltage5V.output5v() == 5) {
      System.out.println("can charging");
    } else {
      System.out.println("can not charging");
    }
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Voltage5V voltage5V = new Adapter(new Voltage220());
    Phone phone = new Phone();
    phone.charg(voltage5V);
  }
}
{% endhighlight %}
```
can charging
```