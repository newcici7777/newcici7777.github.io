---
title: 檔案串流
date: 2025-05-08
keywords: Java, FileInputStream, FileOutputStream, FileReader, FileWriter
---
## 串流固定寫法
### 為什麼一定要關閉串流？
不關閉串流，資料會存在記憶體，不會寫到檔案裡，有關閉的動作，才會有flush的動作，把資料寫到檔案裡。
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
  // ...避免程式碼過多，截掉
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
  }
  // ..避免程式碼過多，截掉
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
    // 存放每次讀取的數量
    int len = 0;
    byte[] buffer = new byte[3];
    // -1 代表沒有資料讀取完畢
    while ((len = fileInputStream.read(buffer)) != -1) {
      // byte[]轉字串
      System.out.println(new String(buffer));
    }
  }// ...避免程式碼過多，截掉
{% endhighlight %}
```
hel
lol
```
預期應該會出現hel與lo，並非hel與lol。

## FileOutputStream寫檔案
### 建構子
有下列二個建構子，參數為檔案或字串，預設為覆蓋原有檔案，第2個參數是不覆蓋原有檔案，而是從檔案末尾開始寫資料。
{% highlight java linenos %}
FileOutputStream(File file)
FileOutputStream(File file, boolean append)
FileOutputStream(String name)
FileOutputStream(String name, boolean append)
{% endhighlight %}

### write(int)寫一個byte
{% highlight java linenos %}
  FileOutputStream fileOutputStream = null;
  try {
    // 建立檔案輸出串流
    fileOutputStream = new FileOutputStream("/Users/cici/testc/file_test");
    // 寫入輸出串流
    fileOutputStream.write('A');
  }// ...避免程式碼過多，截掉
{% endhighlight %}
```
A
```
### write(byte[])
{% highlight java linenos %}
  // 宣告檔案輸出串流
  FileOutputStream fileOutputStream = null;
  try {
    // 建立檔案輸出串流
    fileOutputStream = new FileOutputStream("/Users/cici/testc/file_test");
    String str = "Hello world!";
    // 字串轉byte陣列
    fileOutputStream.write(str.getBytes());
  }// ...避免程式碼過多，截掉
{% endhighlight %}
```
Hello world!
```
### write(byte[],int,int)
第2個參數，開始位置  
第3個參數，寫入byte數量  
{% highlight java linenos %}
write(byte[] b, int off, int len)
{% endhighlight %}

{% highlight java linenos %}
  FileOutputStream fileOutputStream = null;
  try {
    // 建立檔案輸出串流
    fileOutputStream = new FileOutputStream("/Users/cici/testc/file_test");
    String str = "Hello world!";
    // 寫入輸出串流
    fileOutputStream.write(str.getBytes(), 2, 6);
  }// ...避免程式碼過多，截掉
{% endhighlight %}
```
llo wo
```

## FileInputStream與FileOutputStream
拷貝圖片，以下是邊讀邊寫。

之前提過，每一次讀完，陣列buffer不會被清空，會存著上一次讀取的值，所以一定要使用write(byte[],int,int)，指定寫入的數量，假設最後一次迴圈只有讀到10byte，但buffer[1024]，1024個byte只有前面10個byte是這次讀到的值，而剩下的1014byte都是上一個迴圈舊的資料，如果把1014byte也寫到檔案，會造成檔案損毀，無法開啟。

{% highlight java linenos %}
    // 宣告串流
    FileInputStream fileInputStream = null;
    FileOutputStream fileOutputStream = null;
    try {
      // 建立輸入(讀)串流
      fileInputStream = new FileInputStream("/Users/cici/testc/1.png");
      // 建立輸出(寫)串流
      fileOutputStream = new FileOutputStream("/Users/cici/testc/2.png");
      // 存放每次讀取的數量
      int len = 0;
      byte[] buffer = new byte[1024];
      // -1 代表沒有資料讀取完畢
      while ((len = fileInputStream.read(buffer)) != -1) {
        // 邊讀邊寫
        // 一定要指定寫入數量len
        fileOutputStream.write(buffer, 0, len);
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      // 關閉串流
      try {
        // 關閉讀串流
        fileInputStream.close();
        // 關閉寫串流
        fileOutputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
{% endhighlight %}

## FileReader讀取文字檔
只能讀取文字檔，不能讀取圖片或壓縮檔，可以處理中文字、UTF-8。

### read()讀取字元
- int read() 一次讀一個字元，傳回值為int。  
傳回值是真正的資料，每次讀取完的字元存放在data變數。
{% highlight java linenos %}
  FileReader reader = null;
  try {
    // 檔案內容llo wo
    reader = new FileReader("/Users/cici/testc/file_test");
    // 存放值
    int data = 0;
    // -1 代表沒有資料讀取完畢
    while ((data = reader.read()) != -1) {
      System.out.println((char)data);
    }
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    // 關閉串流
    try {
      reader.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
{% endhighlight %}
```
l
l
o
 
w
o
```

### read(char[])
- int read(char[]) 一次讀取多個字元，假設設定8，一次讀取8字元，並把讀到的資料放在char[]。  
傳回值是讀到的數量。
{% highlight java linenos %}
  FileReader reader = null;
  try {
    // 檔案內容llo wo
    reader = new FileReader("/Users/cici/testc/file_test");
    int len = 0;
    char[] buffer = new char[3];
    while ((len = reader.read(buffer)) != -1) {
      System.out.println(new String(buffer, 0, len));
    }
  }
  // ...避免程式碼過多，截掉
{% endhighlight %}
```
llo
 wo
```

## FileWriter寫到文字檔
只能寫文字檔，不能寫圖片或壓縮檔，可以處理中文字、UTF-8。  
一定要關閉串流，不然不會寫到檔案裡，只會暫存在記憶體。
### write(int c)
寫入一個字元  
{% highlight java linenos %}
  FileWriter writer = null;
  try {
    writer = new FileWriter("/Users/cici/testc/file_test");
    writer.write('A');
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    // 關閉串流
    try {
      writer.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
{% endhighlight %}
```
A
```
### write(char[])
{% highlight java linenos %}
  FileWriter writer = null;
  try {
    writer = new FileWriter("/Users/cici/testc/file_test");
    char[] chars  = {'A', 'B', 'C'};
    writer.write(chars);
  }
{% endhighlight %}
```
ABC
```
### write(String)
輸入中文字串
{% highlight java linenos %}
  FileWriter writer = null;
  try {
    writer = new FileWriter("/Users/cici/testc/file_test");
    writer.write("測試");
  }
{% endhighlight %}
```
測試
```

### write(String,int,int)
{% highlight java linenos %}
  FileWriter writer = null;
  try {
    writer = new FileWriter("/Users/cici/testc/file_test");
    // 從索引2，取2個
    writer.write("測試程式語言", 2, 2);
  }
{% endhighlight %}
```
程式
```