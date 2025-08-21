---
title: 2的補數
date: 2025-06-20
keywords: c++, java, 2's complement, 1's complement
---
2的補數，把負數變成2進位0和1的方法。

電腦「儲存負數」也是儲存「2的補數」。

電腦「計算」的方式都是用「2的補數」來計算。

## 名詞介紹

|英文|中文|
|Sign-and-Magnitude|符號數值表示法|
|1's complement|1的補數|
|2's complement|2的補數|

### 符號數值表示法
「符號數值表示法」就是有正負號的二進位。

把最左邊的第1個bit當作正負號，1為負號，0為正號，剩下的bit就跟原本二進位一樣。

1的符號數值表示法 = 00000001

-1的符號數值表示法 = 10000001

### 1的補數
把整數1的二進位00000001，0與1互換，就是1的補數。
```
00000001
↓
11111110
```

### 2的補數
1的補數 \+ 1 = 2的補數

```
 11111110
+       1
-----------
 11111111
```

11111111 為2的補數。

以上是+1變成-1(2的補數)的過程，電腦負數的計算與儲存，都是存2的補數。

## 正整數
正整數的二進位與「符號數值表示法」、「1的補數」、「2的補數」都一樣，不用再轉換。

正整數最左邊第1個bit是0。

1 符號數值表示法 = <span class="markline">0</span>0000001

1 1的補數 = <span class="markline">0</span>0000001

1 2的補數 = <span class="markline">0</span>0000001

## 轉成2的補數
負數就是2的補數，負數不能再轉2的補數。

只有正整數才能轉2的補數。

要把正整數1變成負-1的過程如下:

```
00000001
   ↓ 
0與1互換
   ↓ 
11111110
   ↓ 
  加1
   ↓ 
11111111
```

## 2的補數轉回符號數值表示法
英文是Convert 2's complement to original binary.

什麼時候要把2的補數轉回「符號數值表示法」？

若計算結果是負數(最左邊第1個bit是1)，負數本身就是2的補數，但電腦顯示負號是用「符號數值表示法」也就是有正負號的二進位來顯示，不是顯示2的補數。

如何轉回符號數值表示法？

2的補數 -> 最左邊第1個bit不變，其它0與1互換 -> 加1 -> 符號數值表示法

```
2的補數 11111111
   ↓ 
最左邊第1個bit不變，其它0與1互換
   ↓ 
10000000
   ↓ 
  加1
   ↓ 
10000001
```
<span class="markline">1</span>0000001

注意！左邊第1個bit是正負號，0代表正整數，1代表負數。

後面7個bit(0000001)才是代表數值1。

### 計算
電腦的計算是用2的補數在計算，所有數字都要轉成2的補數，負數(第1個bit是1)本身就是2的補數，就不用再轉成2的補數。

正整數的二進位與「符號數值表示法」、「1的補數」、「2的補數」都一樣，不用再轉換。

### 顯示
計算是用2的補數在計算，顯示結果在螢幕則是以「符號數值表示法」來顯示。

### 範例1
{% highlight java linenos %}
System.out.println(~ 2);
{% endhighlight %}

2的2進位 = 00000010

\~ 0與1位元互換，位元反轉運算子

```
00000010
   ↓
~ 0與1互換
   ↓
11111101
```

計算結果是11111101是一個負數，第1個bit是1，負數本身就是2的補數。

顯示結果在螢幕則是以「符號數值表示法」來顯示，2的補數要轉回「符號數值表示法」。

2的補數 -> 最左邊第1個bit不變，其它0與1互換 -> 加1 -> 符號數值表示法

```
11111101
   ↓ 
最左邊第1個bit不變，其它0與1互換
   ↓ 
10000010
   ↓ 
  加1
   ↓ 
10000011
```

#### 程式碼
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(~ 2);
  }
}
{% endhighlight %}
```
-3
```
執行結果就是-3

## 2的補數變成程式
要如何寫2的補數的程式碼呢？

前一個例子的2的補數11111101

java程式碼
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 2的補數
    byte b = (byte) 0b11111101;
    // 會自動轉回符號數值表示法
    System.out.println(b);
  }
}
{% endhighlight %}
```
-3
```

c++程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  // 2的補數
  int8_t b = 0b11111101;
  // 會自動轉回符號數值表示法
  cout << (int)b << endl;
  return 0;
}
{% endhighlight %}
```
-3
```

## 計算結果為正整數
題目:
{% highlight java linenos %}
System.out.println(~ -2);
{% endhighlight %}

計算是用2的補數來計算，以下是計算步驟:<br>
1. 先求出-2，也就是正整數2的2補數(負數)。
2. 接下來\~ 0與1位元互換，位元反轉運算子

### 求出-2
正整數2的二進位 = 00000010

以下是正整數變成負數的過程:

2進位 -> 0與1互換 -> 加1 -> 2的補數

```
00000010 
   ↓
0與1互換
   ↓
11111101
   ↓
  加1
   ↓
11111110
```

得到-2(2的補數) = 11111110

### 位元反轉運算子
接下來\~ 0與1位元互換，位元反轉運算子

```
11111110
   ↓
0與1位元互換，位元反轉運算子
   ↓
00000001
```

00000001已經是正整數二進位，左邊第1個bit是0，不用再轉回符號數值表示法。

### 程式碼
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(~ -2);
  }
}
{% endhighlight %}
```
1
```

## 位元OR運算子計算
`|`位元OR運算子，有1個為1，才是1。

題目:<br>
{% highlight java linenos %}
  System.out.println(-2 | 3);
{% endhighlight %}

計算步驟:<br>
1. -2的2的補數，上一個範例已經算出來是11111110。
2. 3是正整數，正整數2的補數與2進位的3都是相同。
3. -2與3，or運算。
4. 若結果為2的補數(第1個bit為1)，要再轉回符號數值表示法。

-2與3進行or運算。
```
   11111110 (-2)
or 00000011 (3)
   ---------
   11111111 
```

or的計算結果為負數(2的補數)，第1個bit是1。

要把計算結果轉成「符號數值表示法」。

2的補數 -> 最左邊第1個bit不變，其它0與1互換 -> 加1 -> 符號數值表示法

```
11111111
   ↓
最左邊第1個bit不變，其它0與1互換
   ↓
10000000
   ↓
  加1
   ↓
10000001
```

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(-2 | 3);
  }
}
{% endhighlight %}
```
-1
```
執行結果為-1。

## 重要公式
正整數二進位變成負數(2的補數)過程:
```
正整數二進位
   ↓
0與1互換(1的補數)
   ↓
  加1
   ↓
2的補數
```

負數(2的補數)變成「符號數值表示法」過程。
```
2的補數
   ↓
最左邊第1個bit不變，其它0與1互換
   ↓
  加1
   ↓
符號數值表示法
```

## 其它注意事項
### 0的符號數值表示法，1的補數，2的補數都一樣

0 符號數值表示法 = 00000000

0 1的補數 = 00000000

0 2的補數 = 00000000

### 8bit的-128
8bit的-128，10000000是2的補數，10000000沒辦法轉回「符號數值表示法」。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // -128
    byte b = (byte) 0b10000000;
    System.out.println(b);
  }
}
{% endhighlight %}

## 符號數值表示法、2的補數
以下是有爭議，容易搞混淆的。

|10進位|符號數值表示法|正整數二進位|1的補數|2的補數|
|:---|:----------|:----------|:----------|:----------|
|-1|10000001|00000001|11111110|11111111|
|-2|10000010|00000010|11111101|11111110|
|-127|11111111|01111111|10000000|10000001|
|-128|無法表示|沒有+128|無法表示|10000000|
|0|00000000|00000000|00000000|00000000|
|1|00000001|00000001|00000001|00000001|

## unsinged
unsinged的意思是沒有正負號的正整數。

C++有unsinged正整數。 

Java的數字都有正負號，沒有unsinged正整數。


## 其它補充:二進位減法
若尾數為0，二進位減法技巧。

從右向左，第一個出現的1變成0，後面所有的0變成1。
```
 10000010
-       1
------------
 10000001
```

```
 10000100
-       1
------------
 10000011
```

```
 10001000
-       1
------------
 10000111
```

```
 10101000
-       1
------------
 10100111
```
## 2進位byte與string互轉
### 2進位2's補數字串轉byte
使用Integer.parseInt(字串,2)
{% highlight java linenos %}
// 1的2's補數為-1
String str = "11111111";
System.out.println((byte)Integer.parseInt(str,2));
{% endhighlight %}
```
-1
```

### byte是負數，轉string二進位
```
Integer.toBinaryString(byte)
```
印出的是整數的大小4byte，1個byte中又有8bit，所以會印出8bit \* 4(因為4byte) = 32個bit的二進位字元。<br>
傳回的仍是2進位2's補數。<br>
{% highlight java linenos %}
// 1的2's補數為-1
String str = "11111111";
byte b = (byte) Integer.parseInt(str, 2);
// 把2's補數為-1的byte，轉成2進位String
String str2 = Integer.toBinaryString(b);
System.out.println("str2 = " + str2);
{% endhighlight %}
```
str2 = 11111111111111111111111111111111
```

### byte是正數，轉string二進位
但是注意！如果是正整數1，不是2's補數，印出來是1，不是32個bit的字元
{% highlight java linenos %}
// 正整數1
String str = "00000001";
// 轉成byte
byte b_test = (byte) Integer.parseInt(str, 2);
// byte轉str
String str2 = Integer.toBinaryString(b_test);
System.out.println("str2 = " + str2);
{% endhighlight %}
```
str2 = 1
```

如果要取得1的二進位字元，要用256(1 0000 0000)做or。
```
   1 0000 0000
or   0000 0001
----------------
   1 0000 0001
``` 
再substring，倒數8位元，取得0000 0001

注意！需把byte轉成int，才能做or。
{% highlight java linenos %}
String str = "00000001";
byte b_test = (byte) Integer.parseInt(str, 2);
// 把byte轉成int
int temp = b_test;
// 與1 0000 0000 進行or
temp |= 256;
// 把二進位轉成字串
String str2 = Integer.toBinaryString(temp);
System.out.println("str2 = " + str2);
// 取出倒數8位元
System.out.println("substring str2 = " + str2.substring(str2.length() - 8));
{% endhighlight %}
```
str2 = 100000001
substring str2 = 00000001
```