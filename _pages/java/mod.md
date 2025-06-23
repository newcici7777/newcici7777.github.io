---
title: 餘數
date: 2025-06-18
keywords: Java, C++, mod
---
國一學的正負數，取得的餘數都是正的。

## 負餘數
但在C++與Java，餘數是有負數的。

例子
{% highlight java linenos %}
public class Test {
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

## 餘數公式
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

參考資料[負數乘除][1]

## 除上小數點
得到的餘數是近似值，就是大約多少，不能作為比較或相等依據。
{% highlight java linenos %}
System.out.println(-10.5 % 3);
System.out.println(-10.4 % 3);
{% endhighlight %}
```
-1.5
-1.4000000000000004
```


[1]: {% link _pages/math/neg_div.md %}
