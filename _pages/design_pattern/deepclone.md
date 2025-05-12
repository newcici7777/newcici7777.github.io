---
title: DeepClone
date: 2025-04-18
keywords: Java, Design patterns, DeepClone, Prototype
---
Prerequisites:

- [ByteArray串流][1]
- [序列化與反序列化][2]

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
    // 寫到記憶體緩衝區
    ByteArrayOutputStream bos = null;
    // 物件裝飾串流
    ObjectOutputStream oos = null;
    // 讀取記憶體緩衝區
    ByteArrayInputStream bis = null;
    // 讀取物件
    ObjectInputStream ois = null;
    try {
      // 建立串流
      bos = new ByteArrayOutputStream();
      oos = new ObjectOutputStream(bos);
      // 物件寫到記憶體緩衝區
      oos.writeObject(this);
      // toByteArray() 將記憶體緩衝區複製到byte[]傳回
      // 建立讀取串流
      bis = new ByteArrayInputStream(bos.toByteArray());
      // 讀取物件
      ois = new ObjectInputStream(bis);
      // readObject()是Object物件
      // 向下轉型成Barbie才能使用Barbie的方法
      Barbie copy = (Barbie) ois.readObject();
      return copy;
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    } finally {
      try {
        // 只需要關閉裝飾串流，詳見裝飾串流文章
        ois.close();
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
    //複製3隻經典芭比
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

[1]: {% link _pages/java/bytearr_io.md %}
[2]: {% link _pages/java/obj_stream.md %}