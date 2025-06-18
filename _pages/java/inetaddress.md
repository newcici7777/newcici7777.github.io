---
title: InetAddress取得ip
date: 2025-06-17
keywords: Java, socket, InetAddress
---
## 取得本機ip
{% highlight java linenos %}
    InetAddress inetAddress = InetAddress.getLocalHost();
    // 取得IP
    System.out.println("ip = " + inetAddress.getHostAddress());
    // 取得主機名
    System.out.println("電腦名 = " + inetAddress.getHostName());
{% endhighlight %}
```
ip = 127.0.0.1
電腦名 = xxxx.local
```

## 透過域名取得ip
### DNS
DNS(domain name server)，也稱為域名。

IP太長記不住，網際網路的設計者發明了DNS，DNS可以把伺服器主機名字與IP對映起來，伺服器最少會有一個主機名字。

即便伺服器的IP改變，但伺服器主機名不會改變，還是可以對映到改變後的IP。

語法
{% highlight java linenos %}
InetAddress addr = InetAddress.getByName("DNS域名");
{% endhighlight %}

{% highlight java linenos %}
    InetAddress addr = InetAddress.getByName("www.google.com");
    System.out.println(addr);
    // 取得ip
    System.out.println(addr.getHostAddress());
{% endhighlight %}
```
www.google.com/142.250.196.196
142.250.196.196
```

