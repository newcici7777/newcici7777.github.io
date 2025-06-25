---
title: float與double
date: 2025-06-23
keywords: Java, float, double
---
浮點數是可以表示小數的基本型態。

浮點數分為float與double。

|名稱|byte數|
|:--:|:--:|
|float |4 byte|
|double|8 byte|

## float
數字後面加上f或F。

float宣告與初始化。
{% highlight java linenos %}
float f1 = 123.123f;
float f2 = 123.123F;
{% endhighlight %}

### 只能保留小數點後7位
第7位會4捨5入。
{% highlight java linenos %}
float f1 = 1.123456789f;
System.out.println(f1);
{% endhighlight %}
```
1.1234568
```

## double
### 預設值
預設是double基本型態。

以下程式碼，123.55看到小數點，預設是double型態。
{% highlight java linenos %}
System.out.println(123.55);
{% endhighlight %}

### 宣告與初始化
數字後面沒有f，也沒有F，是空的，代表double。
{% highlight java linenos %}
double d1 = 123.55;
{% endhighlight %}

### 自動轉型與強制轉型
可以把float指派給double，之前在整數時有提過，可以把小的型態塞到大的型態中，編譯器會自動轉型。

以下把float型態塞到double型態。
{% highlight java linenos %}
double d1 = 123.55f;
{% endhighlight %}

double型態塞到float型態要強制轉型，因為float只有4byte，double是8byte。
{% highlight java linenos %}
float f1 = (float) 123.55;
{% endhighlight %}

### 計算時自動轉型
計算時，若型態是double，計算結果為int，也會自動轉成double。
{% highlight java linenos %}
int i = 10 / 4;
System.out.println(i);
double d = 10 / 4;
System.out.println(d);
{% endhighlight %}
```
2
2.0
```

若計算的數字有各種型態，自動轉型成裡面中的最大型態。

下面程式碼10為int，4.0為double。
{% highlight java linenos %}
System.out.println(10 / 4.0);
{% endhighlight %}
```
2.5
```

### 小數點精準度比float高
建議使用double，因為float只能保留小數點7位，第7位會4捨5入。
{% highlight java linenos %}
float f1 = 1.123456789f;
double d1 = 1.123456789101112131415;
System.out.println(f1);
System.out.println(d1);
{% endhighlight %}
```
1.1234568
1.1234567891011122
```

### 近似值
人自己算結果為2.7

電腦不是人，它會給出近似值，接近2.7。
{% highlight java linenos %}
double d2 = 8.1 / 3;
System.out.println(d2);
{% endhighlight %}
```
2.6999999999999997
```

在Java使用小數，要進行比較大小，要十分小心。

以下是比較的正確寫法。
{% highlight java linenos %}
double d3 = 2.7;
double d2 = 8.1 / 3;
if(Math.abs(d3 - d2) < 0.00001) {
  System.out.println("相等");
}    
{% endhighlight %}

以下是比較的「錯誤寫法」。
{% highlight java linenos %}
double d3 = 2.7;
double d2 = 8.1 / 3;
if(d3 == d2) {
  System.out.println("相等");
}    
{% endhighlight %}
### 0.55可以不用寫0
可以不用寫0，以下程式碼是正確的。
{% highlight java linenos %}
  float f1 = .55f;
  double d1 = .55;
  System.out.println(f1);
  System.out.println(d1);
{% endhighlight %}
```
0.55
0.55
```
### 指派整數
指派整數給float、double，後面可以有小數點或沒有小數點。
{% highlight java linenos %}
float f1 = 12f;
float f2 = 12.0f;
double d1 = 12;
double d2 = 12.0;
{% endhighlight %}

## 顯示小數位
在螢幕上顯示時，最少都會顯示小數1位。

{% highlight java linenos %}
  float f1 = 12f;
  double d1 = 12;
  System.out.println(f1);
  System.out.println(d1);
{% endhighlight %}
```
12.0
12.0
```

## 科學記號法
科學記號法可以使用e或E，若是float，後面要加上f或F。

1e1 = $ 1 \times 10^{1} $ 

1E2 = $ 1 \times 10^{2} $

1e3 = $ 1 \times 10^{3} $

1e-1 = $ 1 \times 10^{-1} = 1\div10 $

1E-2 = $ 1 \times 10^{-2} = 1 \div 100 $

1E-3 = $ 1 \times 10^{-3} = 1 \div 1000 $

若忘了科學記號法，可查看[十進位系統][1]。

#### 指數為正
{% highlight java linenos %}
public class Type {
  public static void main(String[] args) {
    float f1 = 1e1f;
    float f2 = 1e2f;
    double d1 = 1E1;
    double d2 = 1E2;
    System.out.println(f1);
    System.out.println(f2);
    System.out.println(d1);
    System.out.println(d2);
  }
}
{% endhighlight %}
```
10.0
100.0
10.0
100.0
```

#### 指數為負
{% highlight java linenos %}
public class Type {
  public static void main(String[] args) {
    float f1 = 1e-1f;
    float f2 = 1e-2f;
    double d1 = 1E-1;
    double d2 = 1E-2;
    System.out.println(f1);
    System.out.println(f2);
    System.out.println(d1);
    System.out.println(d2);
  }
}
{% endhighlight %}
```
0.1
0.01
0.1
0.01
```

## 整數、浮點數與String互相轉型
浮點數轉String使用 \+ \"\"，小數點也會跟著顯示出來。
{% highlight java linenos %}
double d5 = 98.0;
String s2 = d5 + "";
System.out.println(s2);
{% endhighlight %}
```
98.0
```

String轉成其它基本型態，每個基本型態都有對映的包裝類別，包裝類別有提供一個parseXX()方法，提供String轉型成某個數字類型。
{% highlight java linenos %}
double d6 = Double.parseDouble("66.6");
float f3 = Float.parseFloat("55.4");
{% endhighlight %}

[1]: {% link _pages/math/carry10.md %}