---
title: DeepClone
date: 2025-04-18
keywords: Java, Design patterns, DeepClone, Prototype
---
芭比工廠要生廠100隻一模一樣的芭比與肯尼，二隻娃娃一起放在盒子中賣。請問如何寫？

Barbie
{% highlight java linenos %}
import java.io.*;

public class Barbie implements Serializable {
  public String name;

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public Ken ken;

  public Ken getKen() {
    return ken;
  }

  public void setKen(Ken ken) {
    this.ken = ken;
  }

  public Barbie deepclone() {
    //資料寫出至目的地，用輸出串流
    ByteArrayOutputStream bos = null;
    ObjectOutputStream oos = null;
    //資料從來源讀取，用輸入串流
    ObjectInputStream ois = null;
    ByteArrayInputStream bis = null;
    try {
      //資料寫出目的地，用輸出串流
      bos = new ByteArrayOutputStream();
      oos = new ObjectOutputStream(bos);
      // 物件寫出至輸出串流
      oos.writeObject(this);
      //寫出完成後，可用toByteArray()取得byte結果
      //資料從來源取出，用輸入串流
      bis = new ByteArrayInputStream(bos.toByteArray());
      ois = new ObjectInputStream(bis);
      // 讀取物件
      Barbie copy = (Barbie) ois.readObject();
      return copy;
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    } finally {
      try {
        bis.close();
        ois.close();
        bos.close();
        oos.close();
      } catch (Exception e) {
        e.printStackTrace();
      }
    }
  }
}
{% endhighlight %}

Ken
{% highlight java linenos %}
public class Ken implements Serializable {
  public String name;
  public String getName() {
    return name;
  }
  public void setName(String name) {
    this.name = name;
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Barbie barbie = new Barbie();
    barbie.setName("經典芭比");
    Ken ken = new Ken();
    ken.setName("Sugar Daddy 肯尼");
    barbie.setKen(ken);
    //生產3隻經典芭比
    Barbie barbie1 = barbie.deepclone();
    System.out.println(barbie1.getName() + "/" + barbie1.getKen().getName()
        + "/hashcode:" + barbie1.hashCode());

    Barbie barbie2 = barbie.deepclone();
    System.out.println(barbie2.getName() + "/" + barbie2.getKen().getName()
        + "/hashcode:" + barbie2.hashCode());

    Barbie barbie3 = barbie.deepclone();
    System.out.println(barbie3.getName() + "/" + barbie3.getKen().getName()
        + "/hashcode:" + barbie3.hashCode());
  }
}
{% endhighlight %}
```
經典芭比/Sugar Daddy 肯尼/hashcode:1673605040
經典芭比/Sugar Daddy 肯尼/hashcode:693632176
經典芭比/Sugar Daddy 肯尼/hashcode:326549596
```