---
title: 變數++
date: 2025-06-18
keywords: Java, Postfix Increment, prefix increment
---
k\+\+與\+\+k，若沒有指派給其它變數，二者都是k = k \+ 1。

但指派給其它變數的話，k\+\+，k在前面，就是把k先跟其它運算子或[運算元][1]計算完後，最後才k = k \+ 1。

\+\+k在後面，就是先k = k \+ 1，再跟其它運算子或[運算元][1]計算。

## \+\+在後面
{% highlight java linenos %}
int k = 8;
int j = k++;
System.out.println("k = " + k + ", j = " + j);
{% endhighlight %}
```
k = 9, j = 8
```

{% highlight java linenos %}
int j = k++;
{% endhighlight %}

等同於

{% highlight java linenos %}
int j = k;
k = k + 1;
{% endhighlight %}

會先把k指派給j，然後k才加1。

## \+\+在前面
{% highlight java linenos %}
 int k = 8;
 int j = ++k;
 System.out.println("k = " + k + ", j = " + j);
{% endhighlight %}
```
k = 9, j = 9
```

{% highlight java linenos %}
 int j = ++k;
{% endhighlight %}

等同於

{% highlight java linenos %}
k = k + 1;
int j = k;
{% endhighlight %}
會先把k加1。

然後指派給j。

## \+\+沒有指派給變數
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

若沒有指派給變數，以下二句都是一模一樣，都是自增。`k = k + 1`
{% highlight java linenos %}
++k;
k++;
{% endhighlight %}

## \+\+給自己
### \+\+在後面
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    int i = 1;
    i = i++;
    System.out.println("i = " + i);
  }
}
{% endhighlight %}
```
i = 1
```

編譯的時候，看到`i = i++;`，指派給自己，程式碼就會變成如下:

1.會先建立一個暫存變數temp，代表最左邊的i。
```
int temp = i;
```

2.自增
```
i = i + 1;
```

3.把暫存變數，指派給i
```
i = temp;
```
### \+\+在前面
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    int i = 1;
    i = ++i;
    System.out.println("i = " + i);
  }
}
{% endhighlight %}
```
i = 2
```

編譯的時候，看到`i = ++i;`，指派給自己，程式碼就會變成如下:

1.先自增。
```
i = i + 1;
```

2.建立一個暫存變數temp，代表最左邊的i。
```
int temp = i;
```

3.把暫存變數，指派給i
```
i = temp;
```

## 其它範例1
{% highlight java linenos %}
int i = 66;
System.out.println(++i+i);
{% endhighlight %}
```
134
```

1.先自增。
```
++i;
↓
i = i + 1;
```
此時 i = 67

2.\+i
\+\+i的結果為67，此時i是67。
```
67 + i
67 + 67
```
134

## 其它範例2
{% highlight java linenos %}
int i = 0, k = 5;
i = -k++;
System.out.println("i = " + i + ", k = " + k);
{% endhighlight %}
```
i = -5, k = 6
```

```
i = -k++;
```
k在前面，先處理k與其它運算子或[運算元][1]。

1.先變負數
```
i = -k;
↓
i = -5;
```
2.k自增
```
k = k + 1;
↓
k = 5 + 1
```


[1]: {% link _pages/java/operator.md %}


