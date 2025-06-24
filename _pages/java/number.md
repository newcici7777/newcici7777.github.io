---
title: 整數
date: 2025-06-23
keywords: Java, byte, short, int, long
---
Prerequisites:

- [byte與bit介紹][1]

Java的整數有下面4種，Java程式碼是在Java虛擬機上執行，不會有不同作業系統型別會有所不同，但C++的[long類型][2]在Win與Linux下是不同的。

為什麼要不同的基本型態，因為記憶體空間有限，不能把任何整數都放在long，這樣是很浪費記憶體空間。

|名稱|byte數|範圍
|:--:|:--:|:-------:|
|byte |1 byte|-128 至 127|
|short|2 byte|$-2 ^{15}$ 至 $2 ^{15} - 1$|
|int  |4 byte|$-2 ^{31}$ 至$2 ^{31} - 1$|
|long |8 byte|$-2 ^{63}$ 至$2 ^{63} - 1$|

## 占據記憶體bit,byte數量
1個Byte由8個bit組成的，下圖為各種基本型態占據記憶體的bit數量。

![img]({{site.imgurl}}/java/type_range.png)

下圖方格子代表1byte，下圖為各種基本型態占據記憶體的byte數量。

![img]({{site.imgurl}}/java/type_range1.png)

下圖中，各種型態都存入127，127是1byte，但對每一種基本型態的意義是不同的，short存入127，short占據記憶體空間是2byte，int存入127，int占據記憶體空間是4byte，long存入127，long占據記憶體空間是8byte。

![img]({{site.imgurl}}/java/type_range2.png)

不同型態占據記憶體空間大小不一樣，雖然存的值都相同。
{% highlight java linenos %}
byte b1 = 127;   // 1 byte
short s1 = 127;  // 2 byte
int i1 = 127;    // 4 byte
long l1 = 127;   // 8 byte
{% endhighlight %}

不能把4個byte的int，存入1個byte的byte，記憶體空間大小不一樣，以下程式碼編譯錯誤。
{% highlight java linenos %}
int i1 = 127;
byte b1 = i1;
{% endhighlight %}

但反過來把1個byte的值存入4個byte中，是可以，從小房子搬到大房子。
{% highlight java linenos %}
byte b1 = 127;
int i1 = b1;
{% endhighlight %}

## 自動轉型
資料型態小的可以塞入大的資料型態，不用手動轉型，編譯器會自動轉型，

byte型態放入int型態。
{% highlight java linenos %}
byte b1 = 127;
int i1 = b1;
{% endhighlight %}

## 強制轉型
資料型態大的塞入小的資料型態，要使用強制轉型，使用圓括號\(轉型的型態\)，程式設計師要負責轉換後精度遺失或超出資料範圍的問題。

int型態放入byte型態。
{% highlight java linenos %}
int i1 = 127;
byte b1 = (byte) i1;
{% endhighlight %}

## 預設整數常數型態
什麼是常數？等號右邊若為數字或字串，這些就是常數。
{% highlight java linenos %}
int i = 123;  // 123是常數
String s = "Hello"  // Hello是常數
{% endhighlight %}

在程式碼任意輸入數字，預設是int。

以下1234是int。
{% highlight java linenos %}
System.out.println(1234);
{% endhighlight %}

## 範圍檢查
整數預設型態是int。

以下程式碼等號右邊是int的「值」，但java編譯器會先檢查範圍，介於範圍之間，就可以設值。
{% highlight java linenos %}
  byte b1 = 127;
  byte b2 = -128;
  short s1 = 32767;
  short s2 = -32768;
  System.out.println(b1);
  System.out.println(b2);
  System.out.println(s1);
  System.out.println(s2);
{% endhighlight %}
```
127
-128
32767
-32768
```

但如果不是「值」，而是基本型態，以下程式碼編譯錯誤。
{% highlight java linenos %}
  int i1 = 127;
  byte b1 = i1;
{% endhighlight %}

## 不能把大的型態塞到小的型態
int是4byte，不能放入short 2byte。
{% highlight java linenos %}
int i1 = 123;
short s1 = i1;
{% endhighlight %}

## byte
byte等號右邊的範圍大小就是-128 至 127，超過就會編譯錯誤。
{% highlight java linenos %}
  byte b1 = 127;
  System.out.println(b1);
  byte b2 = -128;
  System.out.println(b2);
{% endhighlight %}
```
127
-128
```

以下會產生編譯錯誤。
{% highlight java linenos %}
  byte b1 = 128;
{% endhighlight %}

以下會產生編譯錯誤，不能把大的型態塞入小的型態，int是4byte，byte的1個byte。
{% highlight java linenos %}
int i1 = 123;
byte b1 = i1;
{% endhighlight %}

## short
short等號右邊的範圍大小就是-32768 至 32767，超過就會編譯錯誤。
{% highlight java linenos %}
  short s1 = 32767;
  short s2 = -32768;
{% endhighlight %}

## byte、short計算
byte、short計算結果是int，不能把int塞入byte或short。

以下b1 \+ s1是int型態。
{% highlight java linenos %}
  byte b1 = 1;
  short s1 = 1;
  short s2 = b1 + s1;
{% endhighlight %}

以下b1 \+ b2是int型態。
{% highlight java linenos %}
  byte b1 = 1;
  byte b2 = 1;
  byte b3 = b1 + b2;
{% endhighlight %}

以下s1 \+ s2是int型態。
{% highlight java linenos %}
  byte s1 = 1;
  byte s2 = 1;
  byte s3 = s1 + s2;
{% endhighlight %}

## long
常數後面要為l或L，代表為long基本型態。
{% highlight java linenos %}
  long l1 = 10l;
  long l2 = 10L;
  System.out.println(l1);
  System.out.println(l2);
{% endhighlight %}
```
10
10
```

注意！執行結果為10，不是10.0，不要跟double搞混。

## int
int最大的數字是2147483647，若存放的數字超過int最大數字，請使用long。
{% highlight java linenos %}
  int i = 10;
{% endhighlight %}

### 小數無條件捨去
{% highlight java linenos %}
int i = (int)1.9;
System.out.println(i);
{% endhighlight %}
```
1
```

int計算時，只保留整數，小數無條件捨去。
{% highlight java linenos %}
int i = 10 / 4;
System.out.println(i);
{% endhighlight %}
```
2
```

[1]: {% link _pages/c/basic/typicalRange.md %}
[2]: {% link _pages/c/basic/typedef.md %}
