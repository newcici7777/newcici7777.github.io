---
title: Lambda
date: 2025-05-12
keywords: Java, lambda, Functional Interface
---
Prerequisites:

- [介面][1]
- [內部類別][2]
- [匿名類別][3]

介面只有一個抽象方法，可使用Lambda簡化實作介面的語法。

Lambda又稱Functional Interface函式介面。

## 實作Interface的各種方式
{% highlight java linenos %}
public interface Fly {
  void fly();
}
{% endhighlight %}

### 靜態內部類別
{% highlight java linenos %}
public class Test2 {
  static class Bird implements Fly {
    @Override
    public void fly() {
      System.out.println("Bird fly");
    }
  }

  public static void main(String[] args) {
    Fly bird = new Bird();
    bird.fly();
  }
}
{% endhighlight %}

### 區域內部類別
{% highlight java linenos %}
  public static void main(String[] args) {
    class Balloon implements Fly {
      @Override
      public void fly() {
        System.out.println("氣球飛");
      }
    }
    Fly balloon = new Balloon();
    balloon.fly();
  }
{% endhighlight %}

### 匿名類別
{% highlight java linenos %}
  Fly mosquito = new Fly() {
    @Override
    public void fly() {
      System.out.println("蚊子飛");
    }
  };
  mosquito.fly();
{% endhighlight %}

## Lambda
把上面的程式碼，留下括號()與花括號{}，其它全部去掉。

~~new Fly() {~~  
    ~~@Override~~  
    ~~public void fly~~() {  
      System.out.println("蚊子飛");  
    }  
~~};~~  

括號()與花括號{}之間有一個箭頭 -> 作為區隔
{% highlight java linenos %}
  Fly helicopter = () -> {
    System.out.println("直升機飛");
  };
  helicopter.fly();
{% endhighlight %}

若花括號{}主體(body)只有一行程式，可以去掉花括號。
{% highlight java linenos %}
  Fly helicopter = () -> System.out.println("直升機飛");
  helicopter.fly();
{% endhighlight %}

## 有參數的Lambda
Interface加上時速參數，時是hour，速度是speed
{% highlight java linenos %}
public interface Fly {
  void fly(float km, int hour);
}
{% endhighlight %}

{% highlight java linenos %}
  Fly helicopter = (float km, int hour) -> {
    System.out.println("直升機飛 " + hour + " 小時 " + km + "公里");
  };
  helicopter.fly(600, 3);
{% endhighlight %}

去掉參數類型
{% highlight java linenos %}
  Fly helicopter = (km, hour) -> {
    System.out.println("直升機飛 " + hour + " 小時 " + km + "公里");
  };
  helicopter.fly(600, 3);
{% endhighlight %}

若參數只有一個，也可以去掉括號()
{% highlight java linenos %}
  Fly helicopter = km -> System.out.println("直升機飛 " + km + "公里");
  helicopter.fly(600);
{% endhighlight %}

[1]: {% link _pages/java/interface.md %}
[2]: {% link _pages/java/innerclass.md %}
[3]: {% link _pages/java/anonymous_class.md %}
