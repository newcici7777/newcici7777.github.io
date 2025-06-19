---
title: 變數++
date: 2025-06-18
keywords: Java
---
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
i = i+1;
```

2.建立一個暫存變數temp，代表最左邊的i。
```
int temp = i;
```

3.把暫存變數，指派給i
```
i = temp;
```
