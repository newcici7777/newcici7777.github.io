---
title: 二維陣列
date: 2025-07-01
keywords: Java, Two dimensional Arrays
---
二維陣列，就是一維陣列的值，是一維陣列，也就是一維陣列包著多個一維陣列。

二維陣列是由多個一維陣列所組成的。

## 宣告
有以下三種方式。
```
類別[][] 變數;
類別 變數[][];
類別[] 變數[];
```

## 預設值
1. 若為基本型態，初始值為基本型態預設值。
2. 若為類別或陣列，預設值為null。

|基本型態|預設值|
|byte|0|
|short|0|
|int|0|
|long|0|
|float|0.0f|
|double|0.0|
|char|\u0000|
|boolean|false|

## 建立陣列大小
```
類別[][] 變數 = new 類別[大小][大小];
```

注意！大小設定之後，就不能再修改陣列的大小。

2維陣列大小可以不用填，之後再手動分配大小。
```
類別[][] 變數 = new 類別[大小][];
```

注意！大小是從1開始，外圍陣列至少要有1個大小。
```
類別[][] 變數 = new 類別[1][];
```

以下是錯誤寫法，大小不可為0，大小不是陣列索引。
```
類別[][] 變數 = new 類別[0][0];
```

## 初始值
```
類別[][] 變數 = { {值1, 值2, 值3}, {值1, 值2, 值3} };
```
## 程式碼
{% highlight java linenos %}
// 宣告
int[][] arr1;
int arr2[][];
// 建立陣列，分配陣列大小
arr1 = new int[2][3];
arr2 = new int[3][];
// 宣告與建立陣列，只分配1維陣列大小，2維陣列大小沒分配
int[] arr3[] = new int[2][];
// 宣告與初始值建立陣列
int[] arr4[] = { {1, 2, 3}, {4, 5, 6} };
{% endhighlight %}

## 行列
台灣的row是列。<br>
台灣的column是行。<br>
台灣的列row橫向，行column是直向。<br>
大陸的行與列與台灣的是顛倒。<br>
```
二維陣列[row][column] = 值;
二維陣列[列][行] = 值;
二維陣列[2][3] = 1;
```

## 索引範圍
索引範圍是0..(大小 - 1), 0 <= i <= (大小 - 1) <br>
{% highlight java linenos %}
int[] arr3[] = new int[2][3];
{% endhighlight %}

外圍的1維索引範圍為0..(2-1), 0 <= i <= 1。<br>
裡面的1維索引範圍為0..(3-1), 0 <= i <= 2。<br>
{% highlight java linenos %}
int[] arr3[] = new int[2][3];
// 外圍1維陣列索引0與1
// 裡面1維陣列索引0,1,2
arr3[0][0] = 1;
arr3[0][1] = 2;
arr3[0][2] = 3;
arr3[1][0] = 4;
arr3[1][1] = 5;
arr3[1][2] = 6;
{% endhighlight %}

## 2維陣列的值只能為1維陣列
以下的程式碼將編譯錯誤，因為100不是1維陣列。
{% highlight java linenos %}
int[] arr4[] = { {1, 2, 3}, 100};
{% endhighlight %}

## 迴圈
因為int基本型態預設值為0，印出的值為0。<br>
二維陣列就像棋盤一樣擺放。<br>
{% highlight java linenos %}
int[][] arr1 = new int[2][3];
// i的範圍介於0 .. 1 , 0 <= i <= 1，i包含1
for (int i = 0; i < arr1.length; i++) {
  // j的範圍介於0 .. 2, 0 <= j <= 2，j包含2
  for (int j = 0; j < arr1[i].length; j++) {
    System.out.print("[" + i + "][" + j + "]= "
        + arr1[i][j] + "\t");
  }
  // 斷行
  System.out.println();
}
{% endhighlight %}
```
[0][0]= 0	[0][1]= 0	[0][2]= 0	
[1][0]= 0	[1][1]= 0	[1][2]= 0	
```

## Memory model
- [Memory Model][1]

陣列存的是1維陣列的記憶體位址。<br>
![img]({{site.imgurl}}/java/assign_arr2d1.png)

![img]({{site.imgurl}}/java/assign_arr2d2.png)

## 基本型態預設值

|基本型態|預設值|
|byte|0|
|short|0|
|int|0|
|long|0|
|float|0.0f|
|double|0.0|
|char|\u0000|
|boolean|false|

boolean預設值為false
{% highlight java linenos %}
boolean[][] arr1 = new boolean[2][3];
for (int i = 0; i < arr1.length; i++) {
  for (int j = 0; j < arr1[i].length; j++) {
    System.out.println("[" + i + "][" + j + "]= " + arr1[i][j]);
  }
}
{% endhighlight %}
```
[0][0]= false
[0][1]= false
[0][2]= false
[1][0]= false
[1][1]= false
[1][2]= false
```

{% highlight java linenos %}
char[][] arr1 = new char[2][3];
for (int i = 0; i < arr1.length; i++) {
  for (int j = 0; j < arr1[i].length; j++) {
    System.out.println("[" + i + "][" + j + "]= " + arr1[i][j]);
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/java/char_default.png)

## 二維陣列的大小可以不相同
{% highlight java linenos %}
int[][] arr1 = new int[2][];
// 索引0的一維陣列大小為2
arr1[0] = new int[2];
// 索引1的一維陣列大小為5
arr1[1] = new int[5];
{% endhighlight %}

以下的程式碼不會有任何問題，arr4[2]的大小為1個。
{% highlight java linenos %}
int[] arr4[] = { {1, 2, 3}, {4, 5, 6}, {100} };
{% endhighlight %}

但以下程式碼會編譯錯誤，因為值一定要為一維陣列。
{% highlight java linenos %}
int[] arr4[] = { {1, 2, 3}, {4, 5, 6}, 100 };
{% endhighlight %}

手動分配大小
{% highlight java linenos %}
int[][] arr1 = new int[3][];
for (int i = 0; i < arr1.length; i++) {
  // 根據 i 的值來分配大小，大小最少要有1個，所以用i+1
  arr1[i] = new int[i + 1];
  for (int j = 0; j < arr1[i].length; j++) {
    System.out.print("[" + i + "][" + j + "]= "
        + arr1[i][j] + "\t");
  }
  // 斷行
  System.out.println();
}
{% endhighlight %}
```
[0][0]= 0	
[1][0]= 0	[1][1]= 0	
[2][0]= 0	[2][1]= 0	[2][2]= 0
```

## Pascal's Triangle
以下是大小不一致的二維陣列。
```
1
11
121
1331
14641
```
1. 二維陣列大小不一致，使用`new int[4][]`
2. 每一列row的索引`[0]`與索引最末尾`[len - 1]`都為1。
3. 每一列row的索引不為0與len-1，它的值是「上一列的前一行column」與「上一列的本身的同行column」的值相加。

{% highlight java linenos %}
int[][] arr1 = new int[4][];
for (int i = 0; i < arr1.length; i++) {
  // 根據 i 的值來分配大小，大小最少要有1個，所以用i+1
  arr1[i] = new int[i + 1];
  for (int j = 0; j < arr1[i].length; j++) {
    // 索引[0]與索引最末尾[len - 1]都為1
    if (j == 0 || j == arr1[i].length - 1) {
      arr1[i][j] = 1;
    } else {
      // 它的值是「上一列的前一行」與「上一列的本身的同行」的值相加
      arr1[i][j] = arr1[i - 1][j] + arr1[i - 1][j - 1];
    }
    System.out.print(arr1[i][j] + "\t");
  }
  // 斷行
  System.out.println();
}
{% endhighlight %}
```
1	
1	1	
1	2	1	
1	3	3	1
```

## 易混淆的宣告
以下程式碼，arr1是一維陣列，arr2是二維陣列。
{% highlight java linenos %}
int[] arr1,arr2[];
{% endhighlight %}

假設arr1與arr2都已經建立陣列並設定大小，以下程式碼都是編譯錯誤。
{% highlight java linenos %}
arr1[0] = arr2;
arr2[0][0] = arr1;
arr1 = arr2;
{% endhighlight %}

[1]: {% link _pages/java/memory_model.md %}