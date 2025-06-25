---
title: char字元
date: 2025-06-23
keywords: Java, char
---
Prerequisites:

- [字元與Ascii碼][1]
- [ASCII與中文][2]

## 記憶體大小
Java的char與c++的char占據記憶體大小不一樣，Java占據2byte記憶體空間，C++占據1byte記憶體空間。

|名稱|byte數|範圍
|:--:|:--:|:-------:|
|char|2 byte|$-2 ^{15}$ 至 $2 ^{15} - 1$|

## 語法
是使用\'\'，包住字元。
{% highlight java linenos %}
char ch = 'B';
{% endhighlight %}

以下是錯誤寫法，\"\"這個是字串，不是字元。
{% highlight java linenos %}
char ch = "B";
{% endhighlight %}

## 跳脫字元
跳脫字元雖然是用2個字元組成，但對java編譯器來說，跳脫字元視為1個字元。
{% highlight java linenos %}
char ch1 = '\t';
char ch2 = '\n';
{% endhighlight %}

## 數字
若輸入數字97，會顯示97對映的Ascii Code。
{% highlight java linenos %}
char ch = 97;
System.out.println(ch);
{% endhighlight %}
```
a
```

## 顯示Ascii Code
{% highlight java linenos %}
char ch = 'B';
System.out.println((int)ch);
{% endhighlight %}
```
66
```

## Unicode
### 顯示中文Unicode
[Unicode編碼工具](https://tool.chinaz.com/tools/unicode.aspx)

![img]({{site.imgurl}}/java/unicode1.png)

Ascii Code只能支持英語美語，大小只有128，只有正整數沒有負數。

因為要支援其它國家的語言，才會有Unicode，Unicode包含Ascii，中文字佔2byte、英文字佔2byte，但英文字占太多記憶體大小。

utf-8，比Unicode更省記憶體大小，英文佔1byte，中文佔3byte。

{% highlight java linenos %}
char ch = '西';
System.out.println(ch);
// 顯示Unicode
System.out.println((int)ch);
{% endhighlight %}
```
西
35199
```

### Unicode轉中文
將Unicode指派給char變數，會顯示Unicode表對映的中文字。
{% highlight java linenos %}
char ch = 35199;
System.out.println(ch);
{% endhighlight %}
```
西
```

## char計算
以下的int 1，不需要強制轉型成char，在指派數字給char時，會先檢查「範圍大小」，若符合範圍，編譯器會自動轉型。
{% highlight java linenos %}
char ch = 'a' + 1;
System.out.println(ch);
System.out.println((int)ch);
{% endhighlight %}
```
b
98
```

## 強制轉型
以下把int強制轉型成char。
{% highlight java linenos %}
int i2 = 98;
char ch2 = (char) i2;
System.out.println(ch2);
{% endhighlight %}
```
b
```

## char指派給其它基本型態
char無法指派給byte與short。

char是2byte，可以指派給int(4byte), long(8byte), float(4byte), double(8byte)。
{% highlight java linenos %}
char ch3 = 'b';
int i3 = ch3;
long l1 = ch3;
float f = ch3;
double d4 = ch3;
System.out.println(i3);
System.out.println(l1);
System.out.println(f);
System.out.println(d4);
{% endhighlight %}
```
98
98
98.0
98.0
```

char可以與其它型態計算。
{% highlight java linenos %}
char ch3 = 'b';
double d4 = 1.8 + 2.3f + ch3;
System.out.println(d4);
{% endhighlight %}
```
102.09999995231628
```

## String轉成char
String 轉 char，使用charAt()
{% highlight java linenos %}
char c1 = "Hello".charAt(1);
System.out.println(c1);
{% endhighlight %}
```
e
```


[1]: {% link _pages/c/basic/charType.md %}
[2]: {% link _pages/c/string/chineseAscii.md %}
