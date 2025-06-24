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

## 除上小數，餘數是小數
{% highlight java linenos %}
System.out.println(-10.5 % 3);
System.out.println(-10.4 % 3);
{% endhighlight %}
```
-1.5
-1.4000000000000004
```

有疑惑的是第二個，透過公式
```
a - (a/b) * b
-10.4 - (-10.4 / 3) * 3
```

先分步驟拆解:

1.整數相除，沒有餘數
```
-10.4 / 3 = 3
```
2.先乘除，後加減
```
3 * 3 = 9
```
3.結果是-1.4
```
-10.4 - 9 = -1.4
```

計算結果為-1.4，為什麼程式的執行結果是-1.4000000000000004呢？
因為電腦不是人，它以為-10.4後面還有小數-10.4x，所以結果會是-1.4000000000000004


[1]: {% link _pages/math/neg_div.md %}
