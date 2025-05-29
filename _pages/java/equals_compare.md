---
title: ==與equals
date: 2025-05-30
keywords: Java, Object equals
---
Prerequisites:

- [基本型態與包裝類別][1]
- [Java Memory Model][2]

## 比較==
### 基本型態
基本型態是int,short,long,char,boolean,double,float,byte

對於基本型態來說，二個等於==是比較「值」。

以下程式碼是比較10與20，二個數值是否相同。
{% highlight java linenos %}
int i1 = 10;
int i2 = 20;
if (i1 == i2) {
  // do something
}
{% endhighlight %}

### 類別
對於類別來說，二個等於==是比較「記憶體位址」。
{% highlight java linenos %}
Integer i1 = 10;  // 使用自動裝箱
Integer i2 = 20;  // 使用自動裝箱
if (i1 == i2) {
  // do something
}
{% endhighlight %}

## Object equals()
Object是所有類別的父類別，它的equals()是比較二個物件的記憶體位址是否相同。

{% highlight java linenos %}
public boolean equals(Object obj) {
  return (this == obj);
}
{% endhighlight %}

## String equals()
String繼承Object，覆寫equals()方法。

{% highlight java linenos %}
public boolean equals(Object anObject) {
  // 如果傳進來的物件與本身物件記憶體位址相同，代表是同一個物件
  if (this == anObject) {
    return true;
  }
  // 如果傳進來的物件是String類型
  if (anObject instanceof String) {
    String anotherString = (String)anObject;
    // value變數是本身用來儲存字串
    int n = value.length;
    // 如果傳進來的物件value長度與本身value相同
    if (n == anotherString.value.length) {
      char v1[] = value;
      char v2[] = anotherString.value;
      int i = 0;
      while (n-- != 0) {
        // 迴圈比較二者char陣列中的ascii code是否相同
        if (v1[i] != v2[i])
            return false;
        i++;
      }
      return true;
    }
  }
  return false;
}
{% endhighlight %}

## Integer equals
使用拆箱，把物件轉成基本型態，再來比較數值是否相同。
{% highlight java linenos %}
public boolean equals(Object obj) {
  // 傳進來的物件是否為Integer
  if (obj instanceof Integer) {
    // 使用拆箱，進行數值比較
    return value == ((Integer)obj).intValue();
  }
  return false;
}
{% endhighlight %}

[1]: {% link _pages/java/wrap.md %}
[2]: {% link _pages/java/memory_model.md %}