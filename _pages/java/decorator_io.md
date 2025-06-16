---
title: 裝飾串流
date: 2025-05-08
keywords: Java, Decorating IO Streams
---
Prerequisites:
- [串流基礎概念][1]
- [IO裝飾者模式][2]

## 有附加功能的串流

原文: adds functionality to stream

意思是在以下8種串流基礎上，增加其它功能的串流，稱作裝飾串流。

串流讀寫格式分為byte位元組、字元。

byte位元組串流

|讀寫位置|輸入(讀)|輸出(寫)|
|檔案|FileInputStream|FileOutputStream|
|記憶體緩衝區|ByteArrayInputStream|ByteArrayOutputStream|

字元串流

|讀寫位置|輸入(讀)|輸出(寫)|
|檔案|FileReader|FileWriter|
|記憶體緩衝區|CharArrayReader| CharArrayWriter|

## 裝飾串流
裝飾串流指的是有附加功能串流，並非以上8種串流。

裝飾串流建構子必須傳入其它串流才能使用。

有很多裝飾串流，目前只想說明下列幾種，下圖已經把裝飾串流的功能寫上。

byte串流

![img]({{site.imgurl}}/java/decorator_io1.png)

char串流

![img]({{site.imgurl}}/java/decorator_io2.png)

要了解裝飾串流的原理，可以查看[IO裝飾者模式][2]

## 關閉串流
直接關閉最外層的裝飾串流，程式會使用前面提到的8種內部串流(檔案串流或記憶體緩衝區串流)，把串流關閉，內部的串流在原始碼的變數為in。

什麼是最外層的裝飾串流？以下裝飾串流分別為ObjectInputStream、BufferedInputStream，最外層的裝飾串流是ObjectInputStream，最內部串流是FileInputStream。
{% highlight java linenos %}
ObjectInputStream ois = new ObjectInputStream(new BufferedInputStream(new FileInputStream("/Users/cici/testc/1.dat")));
{% endhighlight %}

以下程式碼，把檔案讀取串流變成一個變數fis，裝飾串流buff_in變數，關閉串流不用把二個close，直接關閉最外層的裝飾串流buff_in。
{% highlight java linenos %}
  FileInputStream fis = null;
  // 把位元組資料存在記憶體緩衝區
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


[1]: {% link _pages/java/io.md %}
[2]: {% link _pages/design_pattern/decorator_io.md %}