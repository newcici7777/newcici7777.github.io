---
title: Metadata
date: 2025-06-05
keywords: java, class loader, metadata, metaspace
---
## metadata
類別在ClassLoader載入時，會在Metaspace(Native memory)中，存放metadata，儲存類別的所有資訊，它是屬於C++結構，每一個類別只有一個metadata。

![img]({{site.imgurl}}/java/metadata.png)

## 存放靜態值
就是存靜態變數中的「值」。

## 靜態區塊
靜態區塊是放在靜態方法中。

## Field info
包含類別所有靜態變數名、屬性名。

## Method Info
包含類別中所有方法與建構子。

## vtable
只有override的方法。

final方法與靜態方法、private方法不能覆寫，就沒有在裡面。

## itable
只有實作介面的方法。

## Constant pool
Constant pool是存放編譯後，.class 檔案的內容，中所需要的常數資訊。

### 類別名與package
假設有下面的類別。
{% highlight java linenos %}
package com.example;
public class MyClass {}
{% endhighlight %}

constant pool儲存為
```
#1 = "com/example/MyClass"
```
package套件名由點.轉成\/

### static final
static 靜態 \+ final不可修改 \+ 基本型別，編譯器直接視它為常數，並把常數存在.class檔中，Classloader把類別載入時，就會把常數存到constant pool中。

所以執行下面程式碼，不會呼叫static{}，不會呼叫ClassLoader載入類別。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(WebSite.IMG_URL);
  }
}
class WebSite {
  public final static String IMG_URL = "http://xxxxxx";
  static {
    System.out.println("靜態區塊初始化");
  }
}
{% endhighlight %}

編譯後，就會變成以下內容，不會呼叫WebSite這個類別。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println("http://xxxxxx");
  }
}
{% endhighlight %}

constant pool儲存`http://xxxxxx`
```
#7 = String   #25             // "http://xxxxxx"
#25 = Utf8    http://xxxxxx
```

### String
遇到字串常數，編譯器直接視它為常數，並把常數存在.class檔中。
{% highlight java linenos %}
String str = "hello";
{% endhighlight %}

\"hello\"會被編譯後，存放在constant pool中。

類別載入後，當這些字串被使用時，JVM會從constant pool取出"hello"，放進String Pool，String Pool在jdk8以後都放在Heap記憶體區塊。

