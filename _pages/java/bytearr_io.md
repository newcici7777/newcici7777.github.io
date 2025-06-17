---
title: ByteArray串流
date: 2025-05-12
keywords: Java, ByteArrayInputStream, ByteArrayOutputStream
---
Prerequisites:

- [位元組與字串互轉][1]
- [串流基礎][2]
- [檔案串流][3]

串流分為二類，一種是檔案串流，一種是記憶體緩衝區串流。

記憶體緩衝區串流，就是資料讀取與寫入都在記憶體緩衝區，記憶體緩衝區就是到byte\[\]陣列。

## ByteArrayInputStream
從記憶體緩衝區讀取資料。

建構子，一定要有byte\[\]參數
{% highlight java linenos %}
ByteArrayInputStream(byte[] buf)
{% endhighlight %}

讀取資料
{% highlight java linenos %}
public int read (byte[] b)
{% endhighlight %}
- 參數是存放資料的byte變數
- 讀取完成或沒資料可讀會傳回-1

讀取緩衝區中的資料，與讀取檔案
{% highlight java linenos %}
  ByteArrayInputStream bis = new ByteArrayInputStream("測試測試".getBytes());
  byte[] buffer = new byte[1024];
  int len = 0;
  while((len = bis.read(buffer)) != -1) {
    String s = new String(buffer, 0, len);
    System.out.println(s);
  }
  bis.close();
{% endhighlight %}

## ByteArrayOutputStream
### 建構子
建構子是沒有參數，寫入到記憶體緩衝區。
{% highlight java linenos %}
ByteArrayOutputStream()
{% endhighlight %}

### 方法
#### toByteArray()
將記憶體緩衝區的資料轉成byte陣列。
{% highlight java linenos %}
byte[] toByteArray ()
{% endhighlight %}

使用方法
{% highlight java linenos %}
byte[] data = baos.toByteArray();
{% endhighlight %}

#### toString ()
將位元組串流轉成字串，若沒寫編碼參數，用系統預設的編碼格式，取決於作業系統。
{% highlight java linenos %}
String data = baos.toString();
String data = baos.toString(StandardCharsets.UTF_8);
String data = baos.toString("UTF-8");
{% endhighlight %}

#### write
寫入資料到記憶體緩衝區。
{% highlight java linenos %}
write (byte[] b, int offset, int len)
{% endhighlight %}
- 第1個參數是寫入的資料(類型為byte[])
- 第2個參數是從byte[]中第幾個索引index開始寫
- 第3個參數要寫入多少個數量

寫入byte陣列。
{% highlight java linenos %}
writeBytes (byte[] b)
{% endhighlight %}

寫入char
{% highlight java linenos %}
write (int b)
{% endhighlight %}

#### size()
取得串流大小。
{% highlight java linenos %}
baos.size();
{% endhighlight %}

### 寫入
寫入到記憶體緩衝區，並把緩衝區資料轉為byte陣列。
{% highlight java linenos %}
  ByteArrayOutputStream baos = null;
  try {
    baos = new ByteArrayOutputStream();
    String str = "測試測試";
    // 字串寫入，轉byte[]
    baos.write(str.getBytes());
    // 字串寫入，轉byte[]
    baos.write("哈囉哈囉".getBytes());

    // 將記憶體緩衝區的資料轉成byte陣列
    byte[] data = baos.toByteArray();
    // 將位元組串流轉成字串
    System.out.println(baos.toString());
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      if (baos != null)
        baos.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
{% endhighlight %}
```
測試測試哈囉哈囉
```

## ByteArrayInputStream與ByteArrayOutputStream
將讀取資料從記憶體緩衝區，寫入資料到記憶體緩衝區，二者結合。
{% highlight java linenos %}
public class Test4 {
  public static void main(String[] args) throws IOException {
    // 讀取資料從記憶體緩衝區
    ByteArrayInputStream bis = new ByteArrayInputStream("測試測試".getBytes());
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    byte[] buffer = new byte[1024];
    int len = 0;
    while((len = bis.read(buffer)) != -1) {
      // 寫入資料到記憶體緩衝區
      bos.write(buffer, 0, len);
    }
    // 將位元組串流轉成字串
    System.out.println(bos.toString());
    // 將記憶體緩衝區的資料轉成byte陣列
    byte[] bosData = bos.toByteArray();
    bis.close();
    bos.close();
  }
}
{% endhighlight %}
```
測試測試
```

ByteArrayInputStream的建構子參數也可以是ByteArrayOutputStream
{% highlight java linenos %}
public class Test5 {
  public static void main(String[] args) throws IOException {
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    bos.write("哈囉，你好".getBytes());
    bos.write("測試".getBytes());
    ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
    int len = 0;
    byte[] buffer = new byte[1024];
    while ((len = bis.read(buffer)) != -1) {
      String s1 = new String(buffer, 0, len);
      System.out.println(s1);
    }
  }
}
{% endhighlight %}
```
哈囉，你好測試
```

## InputStream轉成byte陣列工具
可寫成工具，給人使用。
{% highlight java linenos %}
  public static byte[] streamToByteArray(InputStream is) throws IOException {
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    byte[] buffer = new byte[1024];
    int len = 0;
    while((len = is.read(buffer)) != -1) {
      bos.write(buffer, 0, len);
    }
    byte[] byteArr = bos.toByteArray();
    bos.close();
    return byteArr;
  }
{% endhighlight %}

使用方式，可以是文字檔案，也可以是圖片檔案。
{% highlight java linenos %}
public class Test4 {
  public static byte[] streamToByteArray(InputStream is) throws IOException {
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    byte[] buffer = new byte[1024];
    int len = 0;
    while((len = is.read(buffer)) != -1) {
      bos.write(buffer, 0, len);
    }
    byte[] byteArr = bos.toByteArray();
    bos.close();
    return byteArr;
  }
  public static void main(String[] args) throws IOException {
    FileInputStream fis = new FileInputStream("/Users/cici/testc/pic.png");
    byte[] bytes = streamToByteArray(fis);
  }
}
{% endhighlight %}

文字檔
{% highlight java linenos %}
  public static void main(String[] args) throws IOException {
    FileInputStream fis = new FileInputStream("/Users/cici/testc/print_out");
    byte[] bytes = streamToByteArray(fis);
    System.out.println(new String(bytes));
  }
{% endhighlight %}
```
測試測試2
```

[1]: {% link _pages/java/string_io.md %}
[2]: {% link _pages/java/io.md %}
[3]: {% link _pages/java/fileio.md %}