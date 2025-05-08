---
title: 檔案串流
date: 2025-05-08
keywords: Java, FileInputStream, FileOutputStream, FileReader, FileWriter
---
## 串流固定寫法
### 關閉串流
因為串流產生的Error是屬於IOException，強制一定要補捉錯誤，若是Exception或RuntimeException就不用補捉錯誤。

1. 宣告串流要在try...catch...final的外面，若宣告在try{}裡面，變數可見範圍只在try{}裡面，但串流不管有沒有錯誤，最後都一定要在final{}之中，close()關閉串流，宣告在try...catch...final的外面，變數的可見範圍才可以在final{}使用。
2. 一定要關閉串流。
3. 在try{}之中，建立串流。
{% highlight java linenos %}
  // 1.宣告串流
  FileInputStream fileInputStream = null;
  try {
    // 3.建立串流
    fileInputStream = new FileInputStream("/Users/cici/file_test");
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    // 2.關閉串流
    try {
      // 關閉時可能會有錯誤，所以要再catch一次
      fileInputStream.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
  }
{% endhighlight %}

### 自動關閉串流
JDK7有自動關閉串流的功能(Try-with-resources)，不用處理關閉串流，也不能自行撰寫關閉串流，不然會呼叫close()二次(因為你又再寫一次close())。

在try()括號之間，建立串流。
{% highlight java linenos %}
  // 建立串流
  try (FileInputStream fileInputStream = new FileInputStream("/Users/cici/file_test")) {
    
  } catch (IOException e) {
    // 仍是要補捉IOException，有可能檔案沒有
    System.err.println("發生錯誤: " + e.getMessage());
    e.printStackTrace();
  }
{% endhighlight %}

## FileInputStream讀取檔案
### 建構子
有下列二個建構子，參數為檔案或字串。
{% highlight java linenos %}
FileInputStream(File)
FileInputStream(String filename)
{% endhighlight %}

### read()
- int read() 一次讀一個位元組byte，傳回值為資料，型態為int，若值為其它類型，要轉型。

以下範例為每次只讀取一個byte，正常狀況下不會使用這個，因為一次讀一個byte速度很慢。

{% highlight java linenos %}
  // 宣告串流
  FileInputStream fileInputStream = null;
  try {
    // 建立串流
    fileInputStream = new FileInputStream("/Users/cici/testc/file_test");
    int data = 0;
    // -1 代表沒有資料讀取完畢
    while ((data = fileInputStream.read())!= -1) {
      // 強制轉型
      System.out.println((char) data);
    }
  } catch (IOException e) {
  ...避免程式碼過多，截掉
{% endhighlight %}
```
h
e
l
l
o
```
### read(byte[])
- int read(byte[]) 一次讀取多個位元組，假設1024，一次讀取1024個位元組，並把讀到的資料放在byte[]，傳回值為讀到的byte數量。

程式碼
{% highlight java linenos %}
  FileInputStream fileInputStream = null;
  try {
    // 建立串流
    fileInputStream = new FileInputStream("/Users/cici/testc/file_test");
    // 存放讀到的byte數量
    int len = 0;
    byte[] buffer = new byte[1024];
    // -1 代表沒有資料讀取完畢
    while ((len = fileInputStream.read(buffer)) != -1) {
      // 透過String建構子(位元組, 0, 讀到的byte數量)，建立字串
      System.out.println(new String(buffer, 0, len));
    }
  }..避免程式碼過多，截掉
{% endhighlight %}
```
hello
```

值得注意的是，若buffer大小為3，若只使用new String()，沒有指定截取的數量。結果會出錯，因為每一次讀完，陣列buffer不會被清空，會存著上一次讀取的值。
{% highlight java linenos %}
  FileInputStream fileInputStream = null;
  try {
    // 建立串流
    fileInputStream = new FileInputStream("/Users/cici/testc/file_test");
    int len = 0;
    byte[] buffer = new byte[3];
    // -1 代表沒有資料讀取完畢
    while ((len = fileInputStream.read(buffer)) != -1) {
      System.out.println(new String(buffer));
    }
  }
{% endhighlight %}
```
hel
lol
```
預期應該會出現hel與lo，並非hel與lol。

## FileOutputStream寫檔案