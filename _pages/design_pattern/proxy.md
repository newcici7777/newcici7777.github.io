---
title: Proxy代理人模式
date: 2025-05-05
keywords: Java, Design patterns, Proxy Pattern
---
Prerequisites:

- [類別圖][1]

代理人的作用是代替真正的類別，呼叫真正類別的方法。

## 類別圖
![img]({{site.imgurl}}/pattern/proxy1.png)

- Proxy 代理人
- ProxyInterface 代理介面
- RealClass 真正的類別
- Client 測試用的客戶端

代理人要呼叫真正類別的方法，首先，要有一個介面，裡面是要讓代理人呼叫的方法。

代理人與真正的類別都要實作這個代理介面。

代理人的成員屬性也要有真正的類別，本例是使用建構子的方式建立RealClass，所以是屬於無法分開且共生共死的組合關係，菱形是實心。

而客戶端用到代理人，並非用真正的類別。

## 程式碼
ProxyInterface
{% highlight java linenos %}
public interface ProxyInterface {
  void method1();
}
{% endhighlight %}

RealClass
{% highlight java linenos %}
public class RealClass implements ProxyInterface{
  @Override
  public void method1() {
    System.out.println("真正類別要做的事");
  }
}
{% endhighlight %}

Proxy
{% highlight java linenos %}
public class Proxy implements ProxyInterface{
  private RealClass realClass;

  public Proxy() {
    realClass = new RealClass();
  }

  @Override
  public void method1() {
    realClass.method1();
  }
}
{% endhighlight %}

Client
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) {
    Proxy proxy = new Proxy();
    proxy.method1();
  }
}
{% endhighlight %}
```
真正類別要做的事
```

## Java動態代理介面

- [反射][2]

使用反射，傳回代理介面，呼叫代理介面的方法，也可以呼叫到RealClass的方法。

注意！傳回值為介面！不是具體的子類別。

### 類別圖
![img]({{site.imgurl}}/pattern/proxy2.png)

- ProxyFactory 代理工廠，建構子傳入真正類別
- ProxyInterface 代理介面
- RealClass 真正的類別，實作ProxyInterface中的method方法。
- Client 使用ProxyFactory傳回代理介面。

### 程式碼
ProxyInterface與RealClass跟先前的一模一樣。

使用java.lang.reflect.Proxy
```
Proxy.newProxyInstance(classLoader(), Interfaces(), InvocationHandler());
```
- 第一個參數為類別的classLoader
- 第二個參數為類別的所有介面interfaces
- 第三個參數為InvocationHandler()

透過反射的方法，建立代理人

ProxyFactory
{% highlight java linenos %}
// 反射代理人需要的類別
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
public class ProxyFactory {
  // 類型為Object
  private Object realClass;
  
  // 參數類型為Object
  public ProxyFactory(Object realClass) {
    this.realClass = realClass;
  }

  // 傳回值類型是Object
  public Object getProxyInstance() {
    return Proxy.newProxyInstance(
        realClass.getClass().getClassLoader(),  // 取得classloader
        realClass.getClass().getInterfaces(),   // 取得介面
        new InvocationHandler() {
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          	// 第1個參數為realClass真正的類別
            Object instance = method.invoke(realClass, args);
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
    // 類型為ProxyInterface介面，使用子類別RealClass的建構子建立介面
    ProxyInterface realClass = new RealClass();
    // 轉型成ProxyInterface介面，參數代入realClass
    ProxyInterface proxyClass = (ProxyInterface) new ProxyFactory(realClass).getProxyInstance();
    // 使用介面的方法
    proxyClass.method1();
  }
}
{% endhighlight %}
```
真正類別要做的事
```

[1]: {% link _pages/java/uml.md %}
[2]: {% link _pages/java/reflect.md %}