---
title: System.in System.out
date: 2025-06-04
keywords: Java, System.in, System.out
---
## System.in
主要是作為鍵盤輸入串流。

透過getClass()，可以知道System.in的[執行類型][1]是BufferedInputStream。
{% highlight java linenos %}
System.out.println(System.in.getClass());
{% endhighlight %}
```
class java.io.BufferedInputStream
```

## Scanner
Scanner掃描器，它的建構子參數是System.in鍵盤輸入串流。
{% highlight java linenos %}
Scanner scanner = new Scanner(System.in);
{% endhighlight %}

Scanner會去鍵盤輸入串流中取得資料。

使用next()方法取得字串。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    System.out.println("Please input:");
    String next = scanner.next();
    System.out.println(next);
  }
}
{% endhighlight %}

使用nextInt()，取得數字。
{% highlight java linenos %}
int next1 = scanner.nextInt();
{% endhighlight %}

## System.out
執行類型是PrintStream，將資料顯示在螢幕。
{% highlight java linenos %}
System.out.println(System.out.getClass());
{% endhighlight %}
```
class java.io.PrintStream
```


[1]: {% link _pages/java/polymorphism.md %}