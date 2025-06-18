---
title: 餘數
date: 2025-06-18
keywords: Java, C++, mod
---
國一學的正負數，取得的餘數都是正的。

但在C++與Java，餘數是有負數的。

例子
{% highlight java linenos %}
public class UdpRecv {
  public static void main(String[] args) {
    System.out.println(-38 % -6);
    System.out.println(-38 % 6);
    System.out.println(38 % -6);
  }
}
{% endhighlight %}
```
-2
-2
2
```

Java與C++的餘數公式如下:
```
a % b = a - (a / b) * b
```

{% highlight java linenos %}
System.out.println(-38 % -6);
{% endhighlight %}
```
a % b    = a - (a / b) * b
-38 % -6 = -38 - (-38 / -6) * -6
```
答案是-2