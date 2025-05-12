---
title: ByteArray串流
date: 2025-05-12
keywords: Java, ByteArrayInputStream, ByteArrayOutputStream
---
Prerequisites:

- [位元組與字串互轉][1]
- [串流基礎][2]
- [檔案串流][3]

讀取與寫入的位置有二種，分別是檔案與記憶體緩衝區。

根據位置不同，串流分為二類，一種是檔案串流，一種是記憶體緩衝區串流。

所謂的緩衝區串流，就是把資料寫到陣列，至於是那種類型的陣列呢？答案是`byte[]`

## ByteArrayOutputStream
### 建構子
{% highlight java linenos %}
ByteArrayOutputStream()
{% endhighlight %}

### toByteArray()
建立一個`byte[]`陣列，把緩衝區的資料複製進去。
{% highlight java linenos %}
byte[] toByteArray ()
{% endhighlight %}

使用方法
{% highlight java linenos %}
byte[] data = baos.toByteArray();
{% endhighlight %}

### toString ()
將緩衝區的資料轉成字串，若沒寫編碼參數，用系統預設的編碼格式，取決於作業系統。
{% highlight java linenos %}
String data = baos.toString();
String data = baos.toString(StandardCharsets.UTF_8);
String data = baos.toString("UTF-8");
{% endhighlight %}

### write
寫入byte陣列，指定位置與數量。
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

### size()
取得使用的記憶體緩衝區大小。
{% highlight java linenos %}
baos.size();
{% endhighlight %}

### 程式碼
{% highlight java linenos %}
  ByteArrayOutputStream baos = null;
  try {
    baos = new ByteArrayOutputStream();
    String str = "測試測試";
    // 字串寫入，轉byte[]
    baos.write(str.getBytes());
    // 字串寫入，轉byte[]
    baos.write("哈囉哈囉".getBytes());

    // 讀取緩衝區中的資料
    byte[] data = baos.toByteArray();
    String convertData = new String(data);
    System.out.println(convertData);
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

## ByteArrayInputStream
建構子，一定要有byte\[\]參數
{% highlight java linenos %}
ByteArrayInputStream(byte[] buf)
{% endhighlight %}

讀取檔案
{% highlight java linenos %}
public int read (byte[] b)
{% endhighlight %}
- 第1個參數是存放資料的變數
- 讀取完成或沒資料可讀會傳回-1

讀取緩衝區中的資料，與讀取檔案
{% highlight java linenos %}
  bis = new ByteArrayInputStream(baos.toByteArray());
  int len = 0;
  byte[] buffer = new byte[1024];
  while ((len = bis.read(buffer)) != -1) {
    String s1 = new String(buffer, 0, len);
    System.out.println(s1);
  }
{% endhighlight %}

[1]: {% link _pages/java/string.md %}
[2]: {% link _pages/java/io.md %}
[3]: {% link _pages/java/fileio.md %}