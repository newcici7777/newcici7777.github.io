---
title: 遞迴
date: 2025-07-01
keywords: Java, method, recursive
---
Prerequisites:

- [方法][1]

遞迴就是自己呼叫自己，相同的方法呼叫相同的方法。

每呼叫一次方法，就會在Stack建立一個方法記憶體空間，即便是相同的方法，也是建立新的方法記憶體空間，呼叫10次相同方法，就建立10個方法記憶體空間。

## 無傳回值遞迴
請問以下程式碼結果是什麼？
{% highlight java linenos %}
public class Test5 {
  public void method1(int n) {
    if (n > 2) {
      method1(n - 1);
    }
    System.out.println(n);
  }
  public static void main(String[] args) {
    Test5 test5 = new Test5();
    test5.method1(4);
  }
}
{% endhighlight %}
```
2
3
4
```

方法呼叫與print印出、return順序如下圖所示。<br>
![img]({{site.imgurl}}/java/recv.png)

重點:<br>
1. 印出的n，是印出所在方法記憶體空間中的n。
2. 程式中沒有return，但會有隱藏的return。
3. return返回位置是「呼叫方法」的位置。
4. 返回位置後面沒有其它[運算元][2]或[運算子][2]要執行的話，才執行下一行程式碼。

## 有傳回值遞迴
### 程式碼
{% highlight java linenos %}
public class Test4 {
  public int method1(int n) {
    if (n == 1) {
      return n;
    }
    int res = method1(n - 1) + 1;
    return res;
  }
  public static void main(String[] args) {
    Test4 test4 = new Test4();
    int res = test4.method1(4);
    System.out.println(res);
  }
}
{% endhighlight %}
```
4
```
### 方法呼叫
方法呼叫順序如下圖所示。<br>
![img]({{site.imgurl}}/java/recv1.png)

### 傳回值取代方法
每一次返回呼叫位置都會用傳回值去取代~~方法~~ ，下圖中，
~~方法~~ 都會被清除掉，變成傳回值取代~~方法~~ 的位置。<br>
![img]({{site.imgurl}}/java/recv2.png)

## 呼叫遞迴的守則
1.代入的參數是要有逐漸變化(減少或增加)，最後符合停止遞迴條件，離開遞迴。<br>
2.代入的參數都是相同就會變成無限迴圈。<br>
以下程式碼是無限迴圈。<br>
{% highlight java linenos %}
public class Test4 {
  public int method1(int n) {
    if (n == 1) {
      return n;
    }
    // 代入的參數n沒有改變，就會變成無限迴圈
    int res = method1(n) + 1;
    return res;
  }
  public static void main(String[] args) {
    Test4 test4 = new Test4();
    int res = test4.method1(4);
    System.out.println(res);
  }
}
{% endhighlight %}
3.「方法記憶體空間」的參數是「基本型態」「不會」跟「其它方法記憶體空間」共享「基本型態」參數。<br>
4.參數是物件，「會」跟「其它方法記憶體空間」共享。

## 階乘
以前學數學是數字大的在前面。<br>
$5 \times 4 \times 3 \times 2 \times 1$
但現在要反過來，數字小的在前面。<br>

### 程式碼
{% highlight java linenos %}
public class Test6 {
  public int method1(int n) {
    if (n != 1) return method1(n - 1) * n;
    return n;
  }
  public static void main(String[] args) {
    Test6 test6 = new Test6();
    int res = test6.method1(5);
    System.out.println(res);
  }
}
{% endhighlight %}
```
120
```
### 呼叫方法
方法呼叫順序如下圖所示。<br>
![img]({{site.imgurl}}/java/recv3.png)

### 傳回值取代方法
每一次返回呼叫位置都會用傳回值去取代~~方法~~ ，下圖中，
~~方法~~ 都會被清除掉，變成傳回值取代~~方法~~ 的位置。<br>
![img]({{site.imgurl}}/java/recv4.png)

## 費伯納西數
題目:求出費伯納西數，1,1,2,3,5，求出前2項的相加結果<br>

|n  |0|1|2|3|4|5|6|
|傳回|1|1|2|3|5|8|13|

n = 0，傳回1。<br>
n = 1，傳回1。<br>
n = 2，傳回前面2項(n為0與n為1)的值相加。<br>
n = 3，傳回前面2項(n為1與n為2)的值相加。<br>
n = 4，傳回前面2項(n為2與n為3)的值相加。<br>

前面2項的值相加，就是n-1的值 \+ n-2的值。<br>

{% highlight java linenos %}
public class Test8 {
  public int fib(int n) {
    if (n == 0 || n == 1) {
      return 1;
    }
    return fib(n - 1) + fib(n - 2);
  }
  public static void main(String[] args) {
    Test8 test8 = new Test8();
    int res = test8.fib(4);
    System.out.println(res);
  }
}
{% endhighlight %}
```
5
```

![img]({{site.imgurl}}/java/fib.png)

[1]: {% link _pages/java/method.md %}
[2]: {% link _pages/java/operator.md %}
