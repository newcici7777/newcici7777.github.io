---
title: System.in System.out
date: 2025-06-04
keywords: Java, System.in, System.out
---
## System.in
主要是作為鍵盤輸入串流。

透過getClass()，可以知道System.in的[執行類型][1]是BufferedInputStream，Buffered的意思是讀取的位址是在記憶體緩衝區。
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

### PrintStream的write()
PrintStream繼承OutputStream，自然會有父類別的write方法。
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) throws IOException {
    PrintStream out = System.out;
    out.write("測試".getBytes());
    out.close();
  }
}
{% endhighlight %}

### 設定PrintStream輸出文件位置
預設是顯示在顯示器，可以透過建構子，設定輸出的文件位置。
```
PrintStream(String fileName)
```

使用System.setOut
{% highlight java linenos %}
System.setOut(new PrintStream("文件路徑"));
{% endhighlight %}

範例1
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) throws IOException {
    // 使用System.setOut
    System.setOut(new PrintStream("/Users/cici/testc/print_out"));
    PrintStream out = System.out;
    out.write("測試".getBytes());
    out.close();
  }
}
{% endhighlight %}

範例2
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) throws IOException {
    System.setOut(new PrintStream("/Users/cici/testc/print_out"));
    System.out.println("哈囉哈囉");
  }
}
{% endhighlight %}

## PrintWriter

### 建構子是System.out，輸出到螢幕。
注意!使用時一定要close()，因為close()方法是寫入資料，如果沒呼叫close()，螢幕就不會顯示任何東西。
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    PrintWriter writer = new PrintWriter(System.out);
    writer.println("測試測試2");
    // 一定要close
    writer.close();
  }
}
{% endhighlight %}

### 建構子是FileWriter，輸出到檔案。
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) throws IOException {
    PrintWriter writer = new PrintWriter(new FileWriter("/Users/cici/testc/print_out"));
    writer.println("測試測試2");
    // 一定要close
    writer.close();
  }
}
{% endhighlight %}

[1]: {% link _pages/java/polymorphism.md %}