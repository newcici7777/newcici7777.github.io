---
title: Proxy代理人模式
date: 2025-05-05
keywords: Java, Design patterns, Proxy Pattern
---
Prerequisites:

- [類別圖][1]

代理人的作用是代替別人，去做那個人該做的事。

就好像現實世界中，我們買機票是透過旅行社去買，旅行社再跟航空公司買，而不是我們直接去跟航空公司買，但實際付錢的人是我們，不是旅行社。

而旅行社就是代理人，旅客是真正的類別，買機票是方法。

## 類別圖
![img]({{site.imgurl}}/pattern/proxy1.png)

- Agency 旅行社
- BuyTicket 介面
- Tourist 旅客
- Client 測試用的客戶端

首先要有一個介面(BuyTicket)，旅行社Agency與旅客Tourist都要去實作介面pay()方法。

旅行社Agency的成員屬性要有旅客Tourist，使用建構子參數，把旅客傳入旅行社，所以是屬於共生共死的組合關係，菱形是實心。

旅行社Agency的pay()方法，實際上是呼叫旅客的pay()方法。

而測試時，是用旅行社去買機票，不是旅客去買機票。

## 程式碼
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

## Thread
- [執行緒][3]

執行緒也是代理人模式，執行緒都會實作Runnable代理介面。

以下是執行緒代理人。
{% highlight java linenos %}
class ThreadProxy implements Runnable{
  private Runnable target;
  @Override
  public void run() {
    // 真正呼叫的是target的run方法。
    if (target != null) {
      target.run();
    }
  }

  public ThreadProxy(Runnable target) {
    this.target = target;
  }

  // 啟動執行緒
  public void start() {
    start0();
  }

  private void start0() {
    run();
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    // 使用匿名內部類的方法，建立一個匿名的實作Runnable介面的物件
    Runnable r1 = new Runnable() {
      @Override
      public void run() {
        System.out.println("執行r1的run方法");
      }
    };

    // 把real class傳入建構子
    ThreadProxy threadProxy = new ThreadProxy(r1);
    // 真正執行的是匿名內部類別的run方法。
    threadProxy.start();
  }
}
{% endhighlight %}

## Java動態代理介面
- [反射][2]

使用反射，傳回代理介面，呼叫代理介面的方法，也可以呼叫到Tourist的方法。

### 類別圖
![img]({{site.imgurl}}/pattern/proxy2.png)

- ProxyFactory 代理工廠，建構子傳入Tourist
- BuyTicke 介面
- Tourist 
- Client 測試類別

### 程式碼
BuyTicket與Tourist跟先前的一模一樣。

使用java.lang.reflect.Proxy
```
Proxy.newProxyInstance(classLoader(), Interfaces(), InvocationHandler());
```
- 第一個參數為類別的classLoader
- 第二個參數為介面BuyTicket
- 第三個參數為InvocationHandler()

透過反射的方法，建立代理人。

ProxyFactory
{% highlight java linenos %}
// 反射需要的類別
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
public class ProxyFactory {
  // 類型為Object
  private Object tourist;
  
  // 參數類型為Object
  public ProxyFactory(Object tourist) {
    this.tourist = tourist;
  }

  // 傳回值類型是Object
  public Object getProxyInstance() {
    return Proxy.newProxyInstance(
        tourist.getClass().getClassLoader(),  // 取得classloader
        tourist.getClass().getInterfaces(),   // 取得介面
        new InvocationHandler() {
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            // 第1個參數為tourist
            Object instance = method.invoke(tourist, args);
            return instance;
          }
        });
  }
}
{% endhighlight %}

Client
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) {
    // 編譯類型為BuyTicket介面，執行類型Tourist
    BuyTicket tourist = new Tourist();
    // 轉型成BuyTicket介面，參數代入tourist
    BuyTicket agency = (BuyTicket) new ProxyFactory(tourist).getProxyInstance();
    // 呼叫pay方法，但實際上執行Tourist的pay()方法
    agency.pay();
  }
}
{% endhighlight %}
```
旅客提供信用卡 卡號付錢
```

[1]: {% link _pages/java/uml.md %}
[2]: {% link _pages/java/reflect.md %}
[3]: {% link _pages/java/thread.md %}