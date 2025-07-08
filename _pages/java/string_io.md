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

