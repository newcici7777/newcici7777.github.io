---
title: String字串常用方法
date: 2025-05-09
keywords: Java, String Utils
---
## 字串常用方法
### 開頭,結尾,包含
{% highlight java linenos %}
  String s1 = "Hello World";
  // 是否以Hel開頭的？
  boolean isStart = s1.startsWith("Hel");
  System.out.println(isStart);
  // 是否以Hel結尾的？
  boolean isEnd = s1.endsWith("Hel");
  System.out.println(isEnd);
  // 是否包含abc
  boolean isContain = s1.contains("abc");
  System.out.println(isContain);
{% endhighlight %}
```
true
false
false
```
## 尋找字串
{% highlight java linenos %}
  String s1 = "Hello World";
  // 查詢參數在字串中第一次出現的位置，索引值從0開始數
  int position1 = s1.indexOf("ol");
  System.out.println(position1);
  // 從指定的位置開始找
  position1 = s1.indexOf("o", 6);
  System.out.println(position1);
  // 字串最後出現的位置
  position1 = s1.lastIndexOf("ol");
  System.out.println(position1);
{% endhighlight %}
```
3
7
3
```
## 取得字串中的子字串
取得索引大於等於 >= x，小於 < y。<br>
<span class="markline">不包含</span>y。<br>
語法<br>
```
substring(x, y)
x <= 索引 < y
```
程式碼<br>
{% highlight java linenos %}
  String s1 = "Hello World";
  // 取得字串中的子字串
  String str4 = s1.substring(2);
  System.out.println(str4);
  // 取得位置2到6的字元，不包含6
  str4 = s1.substring(2, 6);
  System.out.println(str4);
{% endhighlight %}
```
llo World
llo 
```
## 去掉字串前後
{% highlight java linenos %}
  // trim去掉前後空格
  String str5 = "  Hello World!   ".trim();
  System.out.println(str5);
{% endhighlight %}
```
Hello World!
```
## 字串轉基本類型
int基本類型包了一個殼，就變成類別Interger，就可以使用類別的方法，如Interger.parseInt()字串變成數字
{% highlight java linenos %}
  // 字串轉基本類型
  // 轉int
  int i = Integer.parseInt("12345");
  System.out.println(i);
  // 轉double
  double d = Double.parseDouble("12.55");
  System.out.println(d);
  // 基本類型轉字串
  System.out.println(String.valueOf(i));
  System.out.println(String.valueOf(d));
{% endhighlight %}
```
12345
12.55
12345
12.55
```

## 格式化

|格式符|說明|
|%s|字串|
|%c|字元|
|%d|整數|
|%.2f|小數點，保留小數點2位，會四捨五入|

使用printf()，方法結尾是f。
{% highlight java linenos %}
System.out.printf("姓名:%s,性別:%c,年齡:%d,體重:%.2f","Bill",'男',18,60.559);
{% endhighlight %}
```
姓名:Bill,性別:男,年齡:18,體重:60.56
```

使用String.format()方法，轉成String。
{% highlight java linenos %}
String format = String.format("姓名:%s,性別:%c,年齡:%d,體重:%.2f","Bill",'男',18,60.559);
System.out.println(format);
{% endhighlight %}
```
姓名:Bill,性別:男,年齡:18,體重:60.56
```

{% highlight java linenos %}
String formatStr = "姓名:%s,性別:%c,年齡:%d,體重:%.2f";
String format = String.format(formatStr,"Bill",'男',18,60.559);
System.out.println(format);
{% endhighlight %}