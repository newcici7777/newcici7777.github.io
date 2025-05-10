---
title: 串流基礎概念
date: 2025-05-08
keywords: Java, InputStream, OutputStream, Reader, Writer
---
串流分成輸入串流與輸出串流。
## 輸入串流InputStream
讀取資料到程式裡用輸入串流。

## 輸出串流OutputStream
從程式裡把資料寫出去用輸出串流。

## 串流抽象父類別
串流分二類，一個是用byte位元組來讀取與輸出，另一個是用char字元來讀取與輸出。

### byte位元組
以下都是抽象類別，不能用new關鍵字建立。

![img]({{site.imgurl}}/java/io1.png)

InputStream讀取位元組

OutputStream以「位元組」方式寫出去

### char字元
以下都是抽象類別，不能用new關鍵字建立。

![img]({{site.imgurl}}/java/io2.png)

Reader讀取字元

Writer以「字元」方式寫出去

## 子類別
資料的來源與寫出的位置有二種，一種是檔案，一種是陣列。

### Byte:用byte位元組來讀取與輸出

![img]({{site.imgurl}}/java/io3.png)

|來源|輸入(讀)|輸出(寫)|
|:----|:----|:----|
|檔案|FileInputStream|FileOutputStream|
|記憶體|ByteArrayInputStream|ByteArrayOutputStream|


### 字元:用char字元來讀取與輸出

![img]({{site.imgurl}}/java/io4.png)

|來源|輸入(讀)|輸出(寫)|
|:----|:----|:----|
|檔案|FileReader|FileWriter|
|記憶體|CharArrayReader|CharArrayWriter|

### 檔案
以Byte方式
- FileInputStream讀取檔案，InputStream子類別，可以用new關鍵字建立。
```
java.io.InputStream
 ↳	java.io.FileInputStream
```
- FileOutputStream寫檔案，OutputStream子類別，可以用new關鍵字建立。
```
java.io.OutputStream
 ↳	java.io.FileOutputStream
```

以字元方式
- FileReader讀取檔案，Reader子類別，可以用new關鍵字建立。
```
java.io.Reader
 ↳java.io.InputStreamReader
    ↳	java.io.FileReader
```
- FileWriter寫檔案，Writer子類別，可以用new關鍵字建立。
```
java.io.Writer
 ↳java.io.OutputStreamWriter
    ↳	java.io.FileWriter
```

### 記憶體byte陣列
實際上就是讀寫byte陣列。

ByteArrayInputStream 讀取Byte陣列
```
java.io.InputStream
 ↳	java.io.ByteArrayInputStream
```

ByteArrayOutputStream 寫到Byte陣列
```
java.io.OutputStream
 ↳	java.io.ByteArrayOutputStream
```

建立1024個byte
{% highlight java linenos %}
byte[] bytes = new byte[1024]; 
{% endhighlight %}

建立Byte陣列的輸出串流
{% highlight java linenos %}
ByteArrayOutputStream baos = new ByteArrayOutputStream();
{% endhighlight %}

以上二者都是寫到byte陣列，但使用ByteArrayOutputStream(寫到陣列)，可以使用ObjectOutputStream物件功能，把物件寫到byte陣列。

再透過ByteArrayInputStream讀取陣列與ObjectInputStream物件功能，讀取byte陣列中的物件，實作Deep Clone深拷貝，複製物件。

Deep Clone程式碼
{% highlight java linenos %}
  // 建立byte陣列寫出串流
  ByteArrayOutputStream baos = new ByteArrayOutputStream();
  // 為byte陣列輸出串流 加上物件的功能
  // 原文: adds functionality to output stream
  ObjectOutputStream oos = new ObjectOutputStream(baos);
  // 把物件寫入byte陣列
  // 使用ObjectOutputStream.writeObject()
  oos.writeObject(object);
  
  // 建立byte陣列讀取串流， 資料的來源是byte陣列
  ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());
  // 為byte陣列輸入串流 加上物件的功能
  // 原文: adds functionality to input stream
  ObjectInputStream ois = new ObjectInputStream(bais);
  // 把物件讀取出來
  // 使用ObjectInputStream.readObject()
  return (T) ois.readObject();
{% endhighlight %}	

## 父類別方法
抽象父類別會提供基本的讀取與寫入方法。

### Byte位元組
抽象父類別InputStream、OutputStream

![img]({{site.imgurl}}/java/io1.png)

InputStream
- read()回傳 -1 代表沒有資料讀取完畢。
- int read() 一次讀一個位元組byte，傳回值為資料，型態為int，若值為其它類型，要轉型。
- int read(byte[]) 一次讀取多個位元組，假設1024，一次讀取1024個位元組，並把讀到的資料放在byte[]
- int read(byte[], int start, int len) 第2個參數是從那個陣列索引index開始讀，第3個參數是要讀幾個，並把讀到的資料放在byte[]
- close() 關閉串流

OutputStream
- write(int n) 寫一個位元組。
- write(byte[]) 寫位元組。
- write(byte[], int start, int lne) 第2個參數是從那個陣列索引index開始寫，第3個參數是要寫幾個。
- close() 關閉串流

### 字元
抽象父類別Reader、Writer

![img]({{site.imgurl}}/java/io2.png)

Reader
- read()回傳 -1 代表沒有資料讀取完畢。
- int read() 一次讀一個字元，傳回值為int，值為資料內容，要轉型。
- int read(char[]) 一次讀取多個字元，假設設定8，一次讀取8字元，並把讀到的資料放在char[]。
- int read(char[], int start, int len) 第2個參數是從那個索引index開始讀?第3個參數是要讀幾個，並把讀到的資料放在char[]。
- close() 關閉串流

Writer
- write(int n) 寫一個字元
- write(char[]) 寫入字元陣列
- write(char[], int start, int lne) 第1個參數為寫入的位置，第2個參數是從那個索引index開始寫?第3個參數是要寫幾個。
- close() 關閉串流