---
title: 遞增遞減運算子
date: 2025-06-18
keywords: Java, Increment operator, Decrement operator
---
## 遞增遞減運算子
遞增，就是自己加自己。<br>
遞減，就是自己減自己。<br>

### 沒有指派給其它變數
k\+\+與\+\+k，若沒有指派給其它變數，二者都是k = k \+ 1。

### 有指派給其它變數
#### 變數k在前面，後加加
二種記住的方法，擇一使用。
1. 變數 = k\+\+，k在前面，就是把「k先指派給其它變數」，最後才k加1 k = k \+ 1。
2. 變數 = k\+\+，加加在後面，就是把k先指派給其它變數，「最後才k加1」 k = k \+ 1。

#### 先加加，變數k在後面
二種記住的方法，擇一使用。
1. 變數 = \+\+k，k在後面，就是k先加1 k = k \+ 1，「最後再指派」給其它變數。
2. 變數 = \+\+k，加加在前面，就是「k先加1」 k = k \+ 1，最後再指派給其它變數。

#### 我自己的記法
看是誰在前面，是變數在前，還是加加在前面。
1. 變數在前面，先指派給其它變數。
2. 加加\+\+在前面，先加加。

## k在前面，後加加
先把k指派給j，然後k才遞增k = k \+ 1。
{% highlight java linenos %}
int k = 8;
int j = k++;
System.out.println("k = " + k + ", j = " + j);
{% endhighlight %}
```
k = 9, j = 8
```

`int j = k++;`等同於
{% highlight java linenos %}
int j = k;
k = k + 1;
{% endhighlight %}

## 先加加，k在後面
k先遞增k = k \+ 1，然後指派給j。
{% highlight java linenos %}
 int k = 8;
 int j = ++k;
 System.out.println("k = " + k + ", j = " + j);
{% endhighlight %}
k = 9, j = 9

`int j = ++k;`等同於
{% highlight java linenos %}
k = k + 1;
int j = k;
{% endhighlight %}

## 沒有指派給其它變數
k\+\+與\+\+k，若沒有指派給其它變數，二者都是k = k \+ 1。
{% highlight java linenos %}
int k = 8;
++k;
System.out.println("k = " + k);
k++;
System.out.println("k = " + k);
{% endhighlight %}
```
k = 9
k = 10
```

若沒有指派給變數，以下二句都是一模一樣，都是遞增。`k = k + 1`
{% highlight java linenos %}
++k;
k++;
{% endhighlight %}

## print與遞增
## k在前面，後加加
跟指派的原理一樣，k在前面，先印出k，再`k = k + 1`。
{% highlight java linenos %}
int k = 8;
System.out.println(k++);
{% endhighlight %}
```
8
```

## 先加加，k在後面
跟指派的原理一樣，加加在前面，先`k = k + 1`，再印出k。
{% highlight java linenos %}
int k = 8;
System.out.println(++k);
{% endhighlight %}
```
9
```

## 指派的變數是自己
### k在前面，後加加
k在前面，先指派k給其它變數。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    int k = 1;
    k = k++;
    System.out.println("k = " + k);
  }
}
{% endhighlight %}
```
k = 1
```

編譯的時候，看到`k = k++;`，指派給自己，程式碼就會變成如下:

1.會先建立一個暫存變數temp，k在前面，把k先指派給temp。
```
int temp = k;
```

2.遞增
```
k = k + 1;
```

3.把暫存變數temp的值，指派給k
```
k = temp;
```

### k在後面
加加在前面，先遞增，再把k指派給其它變數。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    int k = 1;
    k = ++k;
    System.out.println("k = " + k);  
  }
}
{% endhighlight %}
```
k = 2
```

編譯的時候，看到`k = ++k;`，指派給自己，程式碼就會變成如下:

1.加加在前面，先遞增。
```
k = k + 1;
```

2.建立一個暫存變數temp，把k變數的值指派給temp。
```
int temp = k;
```

3.把暫存變數temp，指派給k
```
k = temp;
```

## 其它範例1
{% highlight java linenos %}
int k = 66;
System.out.println(++k+k);
{% endhighlight %}
```
134
```

1.先加加，先遞增。
```
++k;
↓
k = k + 1;
```
此時 k = 67

2.再加k，k是67。
```
67 + k
67 + 67
```
134

## 其它範例2
### k在前面，後加加
先把k變成負數，指派給i，然後k才遞增k = k \+ 1。
{% highlight java linenos %}
int i = 0, k = 5;
i = -k++;
System.out.println("i = " + i + ", k = " + k);
{% endhighlight %}
```
i = -5, k = 6
```

`i = -k++;`k在前面，先處理k與其它運算子或[運算元][1]。

1.k變成負數，指派給i
```
i = -k;
↓
i = -5;
```
2.k遞增，注意！此時k仍是5，不是-5。
```
k = k + 1;
↓
k = 5 + 1
```

## 其它範例3
### 變數在前面，後加加
`b++`，變數b在前面，先把b指派給result，然後b才遞增k = k \+ 1。
{% highlight java linenos %}
int a = 10;
int b = 99;
int result = a > b ? a++ : b++;
System.out.println(result);
System.out.println(b);
{% endhighlight %}
```
99
100
```


[1]: {% link _pages/java/operator.md %}


