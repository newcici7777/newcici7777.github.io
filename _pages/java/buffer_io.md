---
title: Buffered串流
date: 2025-05-08
keywords: Java, 
---
## Buffered
Buffered是指把讀取或寫入的資料，保存在記憶體緩衝區中。

## BufferedReader與BufferedWriter
### 建構子
參數只能接收Reader的子類別(處理字元的串流)。
{% highlight java linenos %}
BufferedReader(Reader in)
BufferedWriter(Writer out)
{% endhighlight %}

### readLine()
提供一列一列讀取的功能，讀完資料沒有資料可以讀會傳回null。
{% highlight java linenos %}
// Reads a line of text
String readLine()
{% endhighlight %}

內部使用緩衝buffer功能，提高讀取效率。

{% highlight java linenos %}
  BufferedReader bufferedReader = null;
  try {
    bufferedReader = new BufferedReader(new FileReader("/Users/cici/testc/file_test"));
    String line = null;
    while((line = bufferedReader.readLine()) != null) {
      System.out.println(line);
    }
  }
{% endhighlight %}
```
測試程式

java程式設計
```
### write()與newLine()
write()不會自動換行，需使用BufferedWriter.newLine()才會換行。
{% highlight java linenos %}
  BufferedWriter bufferedWriter = null;
  try {
    bufferedWriter = new BufferedWriter(new FileWriter("/Users/cici/testc/file_test"));
    // 寫一行
    bufferedWriter.write("測試程式");
    // 換行
    bufferedWriter.newLine();
    // 寫一行
    bufferedWriter.write("java程式設計");
    // 換行
    bufferedWriter.newLine();
  }
{% endhighlight %}
```
測試程式
java程式設計
```

## BufferedInputStream與BufferedOutputStream
位元組資料都存在記憶體緩衝區，對緩衝區進行讀取與寫入。
{% highlight java linenos %}
  BufferedInputStream buff_in = null;
  BufferedOutputStream buff_out = null;
  try {
    buff_in = new BufferedInputStream(new FileInputStream("/Users/cici/testc/1.png"));
    buff_out = new BufferedOutputStream(new FileOutputStream("/Users/cici/testc/3.png"));
    // 每次迴圈讀到的byte數
    int len = 0;
    // 建立buffer
    byte[] buff = new byte[1024];
    while ((len = buff_in.read(buff)) != -1) {
      // 寫入
      buff_out.write(buff, 0, len);
    }
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      if (buff_in != null)
        buff_in.close();
      if (buff_out != null)
        buff_out.close();
    } catch (IOException ex) {
      ex.printStackTrace();
    }
  }
{% endhighlight %}
