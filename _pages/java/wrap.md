---
title: 基本型態與Wrapper Classes
date: 2025-05-12
keywords: java, Wrapper Classes, Primitive Type
---
## 基本型態Primitive Type
Java提供基本型態如下

數字相關short,int,long,double,float

字元char

boolean

注意！在Java中，char與boolean跟數字(Number)沒關係。在C++，char與boolean是整數型態，詳細內容請見:

- [C++ char字元][1]
- [C++ bool][1]

## 包裝類別Wrapper Classes
Java還提供對映基本型態的類別，稱為包裝類別。

### 類別圖

![img]({{site.imgurl}}/java/basic_type_extends.png)

由圖中可發現，Boolean與Character不是Number的子類別。

### 包裝類別的組成
#### 組成公式
由Header表頭12byte + 基本型態大小 + 自動補齊(Alignment gap)所組成。

所謂自動補齊，包裝類別是以8byte為一個單位，也就是不是8的倍數，就會自動補齊成8byte的倍數。

例:

Short  12(表頭) + 2(基本型態) = 14

以上相加只有14，並非8的倍數，所以自動補齊2byte，就變成16，16為8的倍數。

Short 12(表頭) + 2(基本型態) + 2(自動補齊) = 16

#### 其它包裝類別的組成
Byte  12(表頭) + 1(基本型態) + 3(自動補齊) = 16

Double 12(表頭) + 8(基本型態) + 4(自動補齊) = 24 要為8的倍數

Integer 12(表頭) + 4(基本型態) + 0(自動補齊) = 16

Character 12(表頭) + 2(基本型態) + 2(自動補齊) = 16

### Java Object Layout
可使用[Java Object Layout][4]物件佈局，查看產生的記憶體大小。

### 基本型態與包裝類別記憶體大小

非Number類型
|基本型態|佔Stack大小|包裝類別|佔Heap大小|
|boolean|1 byte|Byte|16 byte|
|char|2 byte|Character|16 byte|

Number類型
|基本型態|佔Stack大小|包裝類別|佔Heap大小|
|byte|1 byte|Byte|16 byte|
|short|2 byte|Short|16 byte|
|int|4 byte|Integer|16 byte|
|long|8 byte|Long|24 byte|
|float|4 byte|Float|16 byte|
|double|8 byte|Double|24 byte|

### 記憶體位置
什麼是Stack？什麼是Heap？

Stack與Heap都是記憶體空間，Stack放變數與基本型態(char, int, long, float...)，Heap放由類別產生的物件(Character, Integer, Long, Float, Double)。詳見[Java Memory Model][3]

### 基本型態優點
從上面的表格中，可以發現基本型態與包裝類別佔用的記憶體大小差距很大。

基本型態占用記憶體小，不用像包裝類別要建立物件，要被記憶體回收釋放記憶體，int[]陣列比Integer[]陣列更省記憶體空間。

基本型態使用CPU0指令集，使用加法(ADD)減法(SUB)乘(MUL)除(DIV)、XOR、AND、OR...暫存器操作，所以速度很快。

### 為什麼要用包裝類別？
那為什麼使用包裝類別呢？因為基本型態只能做基本運算，如果要用到集合List<Integer>或泛型，或使用包裝類別的方法(如Integer.compareTo())就要用到包裝類別了。

### 基本型態與包裝類別功能分類

|功能分類|基本型態|類別|
|記憶體位置|Stack|Heap|
|記憶體大小|1-8 byte| 16-24 byte|
|GC管理|不用|要記憶體釋放|
|功能|基本運算|支持集合、泛型T型別參數、使用類別方法|
|null|不支持|可以設為null|

### 結論
需要高效數學運算，使用基本型態。

使用物件相關的程式，使用包裝類別。

## 裝箱與拆箱
### 裝箱
裝箱英文是boxing，是把基本型態轉成包裝類別。

語法
```
包裝類別.valueOf(基本型態)
```

{% highlight java linenos %}
// 基本型態
int n1 = 10;

// 基本型態 -> 包裝類別
Integer integer1 = Integer.valueOf(n1);
{% endhighlight %}

### 自動裝箱
jdk1.5時推出自動裝箱(Auto boxing)的功能，不用使用valueOf()的方法。

{% highlight java linenos %}
int n2 = 100;
// 基本資料型態 -> 物件
Integer integer2 = n2;
{% endhighlight %}

### 拆箱
拆箱英文是unboxing，是把包裝類別轉成基本型態。

語法
```
包裝類別.intValue(基本型態)
```

{% highlight java linenos %}
int n1 = 10;
// 基本型態 -> 包裝類別
Integer integer1 = Integer.valueOf(n1);

// 包裝類別 -> 基本資料型態
int i1 = integer1.intValue();
{% endhighlight %}

### 自動拆箱
jdk1.5時推出自動拆箱(Auto unboxing)的功能，不用使用intValue()的方法。

{% highlight java linenos %}
int n1 = 10;
// 基本型態 -> 包裝類別
Integer integer1 = n1;

// 包裝類別 -> 基本資料型態
int i1 = integer1;
{% endhighlight %}

### 自動拆裝箱原理
自動拆箱裝箱，實際上仍是使用valueOf()與intValue()。

打開Integer原始檔，Mac電腦可使用Cmd+滑鼠對著「Integer」按下左鍵，進入原始檔，找到valueOf()方法，加上中斷點。

![img]({{site.imgurl}}/java/wrap1.png)

在自動裝箱的程式碼也加上中斷點。

![img]({{site.imgurl}}/java/wrap2.png)

啟動Debug執行，搭配「step into」，就會發現會進入valueOf()方法。


[1]: {% link _pages/c/basic/charType.md %}
[2]: {% link _pages/c/basic/boolType.md %}
[3]: {% link _pages/java/memory_model.md %}
[4]: {% link _pages/java/obj_layout_core.md %}