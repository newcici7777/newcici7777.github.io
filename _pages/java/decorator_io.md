---
title: 有附加功能的串流
date: 2025-05-08
keywords: Java, 
---
Prerequisites:
- [串流基礎概念][1]
- [IO裝飾者模式][2]

原文: adds functionality to stream

意思是在以下8種串流基礎上，增加其它功能的串流，稱作裝飾串流。

串流讀寫格式分為byte、字元。

byte串流

|讀寫位置|輸入(讀)|輸出(寫)|
|檔案|FileInputStream|FileOutputStream|
|記憶體|ByteArrayInputStream|ByteArrayOutputStream|

字元串流

|讀寫位置|輸入(讀)|輸出(寫)|
|檔案|FileReader|FileWriter|
|記憶體|CharArrayReader|CharArrayWriter|

## 裝飾串流
裝飾串流指的是有附加功能串流，並非以上8種串流。

有很多裝飾串流，目前只想說明下列幾種，下圖已經把裝飾串流的功能寫上。

byte串流

![img]({{site.imgurl}}/java/decorator_io1.png)

char串流

![img]({{site.imgurl}}/java/decorator_io2.png)

要了解裝飾串流的原理，可以查看[IO裝飾者模式][2]

## 關閉串流
直接關閉最外層的裝飾串流，程式會使用最內部的串流(檔案串流或記憶體串流)，把串流關閉，內部的串流在源始碼的變數為in。

什麼是最外層的裝飾串流？以下裝飾串流分別為ObjectInputStream、BufferedInputStream，最外層的裝飾串流是ObjectInputStream。
{% highlight java linenos %}
ObjectInputStream ois = new ObjectInputStream(new BufferedInputStream(new FileInputStream("/Users/cici/testc/1.dat")));
{% endhighlight %}

以下程式碼，把檔案讀取串流變成一個變數fis，裝飾串流buff_in變數，關閉串流不用把二個close，直接關閉最外層的裝飾串流buff_in。
{% highlight java linenos %}
  FileInputStream fis = null;
  BufferedInputStream buff_in = null;
  try {
    fis = new FileInputStream("/Users/cici/testc/1.png");
    buff_in = new BufferedInputStream(fis);
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      // 只要關閉最外面的裝飾串流
      if (buff_in != null)
        buff_in.close();
    } catch (IOException ex) {
      ex.printStackTrace();
    }
  }
{% endhighlight %}

## BufferedReader
### 建構子
參數只能接收Reader的子類別(處理字元的串流)。
{% highlight java linenos %}
BufferedReader(Reader in)
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
使用緩衝區高效讀取與寫入
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

[1]: {% link _pages/java/io.md %}
[2]: {% link _pages/design_pattern/decorator_io.md %}