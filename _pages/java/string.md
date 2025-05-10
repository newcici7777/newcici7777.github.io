---
title: String與char[]和byte[]
date: 2025-05-09
keywords: Java, String
---
建構子
{% highlight java linenos %}
  new String(char[]);
  // 指定開始位置與要拷貝的長度
  new String(char[], int start, int len);
  new String(byte[]);
  // 指定開始位置與要拷貝的長度
  new String(byte[], int start, int len);
{% endhighlight %}

## 字元陣列轉字串
{% highlight java linenos %}
  // 字元陣列轉字串
  char[] charArr = {'A', 'B', 'C'};
  String str2 = new String(charArr);
  System.out.println(str2);
{% endhighlight %}
```
ABC
```
{% highlight java linenos %}
  char[] charArr = {'A', 'B', 'C'};
  // 指定開始位置與要拷貝的長度
  String str3 = new String(charArr, 1, 2);
  System.out.println(str3);
{% endhighlight %}
```
BC
```
## 字串轉字元陣列
{% highlight java linenos %}
  String s1 = "ABCDE";
  // 字串轉字元陣列
  char[] charArr2 = s1.toCharArray();
  for (char c : charArr2) {
    System.out.println(c);
  }
{% endhighlight %}
```
A
B
C
D
E
```
## Byte陣列轉字串
建構子
{% highlight java linenos %}
// 建構子是byte[]
public String (byte[] bytes)
// 從陣列位置offset開始，第2參數是拷貝幾個byte
public String (byte[] bytes, int offset, int length)
{% endhighlight %}

{% highlight java linenos %}
  // byte陣列
  byte[] b = {'A', 'B', 'C'};
  // 轉字串
  String s2 = new String(b);
  System.out.println(s2);
{% endhighlight %}
```
ABC
```

從陣列位置1開始，拷貝2個byte
{% highlight java linenos %}
  // byte陣列
  byte[] b = {'A', 'B', 'C'};
  // 轉字串
  String s3 = new String(b, 1, 2);
  System.out.println(s3);
{% endhighlight %}
```
BC
```
## 字串轉Byte陣列
語法
{% highlight java linenos %}
public byte[] getBytes ()
// 參數可以是字元編碼
public byte[] getBytes (String charsetName)
{% endhighlight %}

{% highlight java linenos %}
  // byte陣列
  byte[] b = {'A', 'B', 'C'};
  // 轉字串
  String s2 = new String(b);
  // 字串轉成byte陣列
  byte[] b2 = s2.getBytes();
  System.out.println("print byte");
  for (byte b1 : b2) {
    System.out.println(b1);
  }
{% endhighlight %}
```
print byte
65
66
67
```

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
{% highlight java linenos %}
  String s1 = "Hello World";
  // 取得字串中的子字串
  String str4 = s1.substring(2);
  System.out.println(str4);
  // 取得位置2到6的字元
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