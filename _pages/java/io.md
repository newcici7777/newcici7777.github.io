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

## 讀取與寫出的位置
資料的讀取與寫出的位置有二種，一種是檔案，一種是記憶體緩衝區。

記憶體緩衝區為電腦暫時置放輸出或輸入資料的位址。

### 用byte來讀取與輸出

![img]({{site.imgurl}}/java/io3.png)

|讀寫位置|輸入(讀)|輸出(寫)|
|:----|:----|:----|
|檔案|FileInputStream|FileOutputStream|
|記憶體|ByteArrayInputStream|ByteArrayOutputStream|


### 用char字元來讀取與輸出

![img]({{site.imgurl}}/java/io4.png)

|讀寫位置|輸入(讀)|輸出(寫)|
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

### 記憶體緩衝區
讀取與寫入的位置是記憶體緩衝區，就像變數在記憶體中會有個位址存放值，二者都是存在記憶體。

ByteArrayInputStream 讀取記憶體緩衝區資料
```
java.io.InputStream
 ↳	java.io.ByteArrayInputStream
```

ByteArrayOutputStream 寫到記憶體緩衝區資料
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

以上二者都是在記憶體建立byte[]陣列的空間，存放資料。

## 父類別方法
抽象父類別會提供基本的讀取與寫入方法。

### Byte位元組
抽象父類別InputStream、OutputStream

![img]({{site.imgurl}}/java/io1.png)

InputStream
- read()傳回 -1 代表沒有資料讀取完畢。
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
- read()傳回 -1 代表沒有資料讀取完畢。
- int read() 一次讀一個字元，傳回值為int，值為資料內容，要轉型。
- int read(char[]) 一次讀取多個字元，假設設定8，一次讀取8字元，並把讀到的資料放在char[]。
- int read(char[], int start, int len) 第2個參數是從那個索引index開始讀?第3個參數是要讀幾個，並把讀到的資料放在char[]。
- close() 關閉串流

Writer
- write(int n) 寫一個字元
- write(char[]) 寫入字元陣列
- write(char[], int start, int lne) 第1個參數為寫入的位置，第2個參數是從那個索引index開始寫?第3個參數是要寫幾個。
- close() 關閉串流