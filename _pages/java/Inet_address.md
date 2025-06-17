---
title: InetAddress取得ip
date: 2025-06-17
keywords: Java, socket, InetAddress
---
## 取得本機ip
{% highlight java linenos %}
    InetAddress inetAddress = InetAddress.getLocalHost();
    System.out.println("ip = " + inetAddress.getHostAddress());
    System.out.println("電腦名 = " + inetAddress.getHostName());
{% endhighlight %}
```
ip = 127.0.0.1
電腦名 = xxxx.local
```