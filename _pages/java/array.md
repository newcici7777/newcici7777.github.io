---
title: 陣列
date: 2025-06-04
keywords: Java, array
---
陣列裡面放是相同類型的物件，一旦初始化或設定大小，就不能再更改陣列大小。
## 語法
### 宣告
```
類別[] 變數;
類別 變數[];
```
\[\]可放在類型後面，也可以放在變數後面，擇一放置。
### 陣列大小
```
類別[] 變數 = new 類別[大小];
```

陣列大小是陣列有多少「個數」，以下程式碼建立3個記憶體空間，索引為0、1、2。
{% highlight java linenos %}
int arr[] = new int[3];
{% endhighlight %}

注意！大小設定之後，就不能再修改陣列的大小。

注意！大小是從1開始，陣列至少要有1個大小。
```
類別[] 變數 = new 類別[1];
```

以下是錯誤寫法，大小不可為0，大小不是陣列索引。
```
類別[] 變數 = new 類別[0];
```
### 初始化語法
```
類別[] 變數 = { 值1, 值2, 值3 };
```
## 同時初始化與建立陣列
一開始就初始化陣列中的值。
{% highlight java linenos %}
char[] arr1 = {'H', 'e', 'l', 'l', 'o'};
int arr2[] = {0, 1, 2, 3};
int[] arr3 = new int[]{1, 2, 3};
{% endhighlight %}

## 建立陣列大小，但並未初始化。
先設定陣列記憶體大小。<br>
{% highlight java linenos %}
int[] arr = new int[3];
int arr[] = new int[3];
{% endhighlight %}

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

{% highlight java linenos %}
int[] arr = new int[3];
for (int i = 0; i < arr.length; i++) {
  System.out.println(arr[i]);
}
{% endhighlight %}
```
0
0
0
```

## 分配陣列的值
{% highlight java linenos %}
int arr[] = new int[3];
arr[0] = 1;
arr[1] = 2;
arr[2] = 3;
{% endhighlight %}

## 索引範圍
索引範圍是0..(大小 - 1)<br>

{% highlight java linenos %}
// 索引範圍是0..1
int[] arr3 = new int[2];
// 1維陣列索引0與1
arr3[0] = 1;
arr3[1] = 2;
{% endhighlight %}

## length陣列大小
取出陣列大小。<br>
注意！！不是arr1.length()，後面沒有圓括號\(\)，length是屬性，不是方法！！請特別注意！
{% highlight java linenos %}
int[] arr1 = {1, 2, 3, 4, 5, 6};
System.out.println(arr1.length);
{% endhighlight %}
```
6
```
## 最後一個元素索引
最後一個元素的索引不是陣列大小，陣列大小跟陣列索引是不同的概念。<br>

陣列大小是陣列有多少「個數」，假設建立大小為3的陣列，索引分別為0、1、2。

以下會編譯錯誤，因為「3」超出陣列索引，索引分別為0、1、2。
{% highlight java linenos %}
int arr[] = new int[3];
arr[3] = 1;
{% endhighlight %}

所以最後一個元素的索引，通常都會這樣寫:「陣列大小」 - 1
{% highlight java linenos %}
int lastIndex = arr.length - 1;
{% endhighlight %}

## 迴圈
### 迴圈判斷
arr1.length陣列大小為4，陣列「最後一個元素」的索引為「陣列大小」 - 1 = 3。<br>
索引範圍為0, 1, 2, 3都小於arr1.length。<br>
以下是最常使用陣列迴圈判斷語法。
{% highlight java linenos %}
i < arr1.length
{% endhighlight %}

### 迴圈語法
{% highlight java linenos %}
int[] arr1 = new int[4];
// i的範圍介於0 .. 3 , 0 <= i <= 3，i包含3。
for (int i = 0; i < arr1.length; i++) {
  System.out.println(arr1[i]);
}
{% endhighlight %}
```
0
0
0
0
```

### 迴圈倒過來語法
arr1.length陣列大小為4，陣列「最後一個元素」的索引為「陣列大小」 - 1 = 3。<br>
最後一個元素的索引，通常都會這樣寫:「陣列大小」 - 1 <br>
{% highlight java linenos %}
int[] arr1 = new int[4];
for (int i = arr1.length - 1; i >= 0; i--) {
  System.out.print(arr1[i]);
}
{% endhighlight %}
```
0000
```

### 迴圈設定陣列的值
{% highlight java linenos %}
int[] arr1 = new int[4];
// i的範圍介於0 .. 3, 0 <= i <= 3，i包含3。
for (int i = 0; i < arr1.length; i++) {
  arr1[i] = i;
  System.out.println(arr1[i]);
}
{% endhighlight %}
```
0
1
2
3
```

## 字元陣列與print
印出字元陣列就是印出字串。
{% highlight java linenos %}
char arr[] = new char[3];
arr[0] = 'A';
arr[1] = 'B';
arr[2] = 'C';
System.out.println(arr);
{% endhighlight %}
```
ABC
```

## char陣列字元運算與迴圈
以下這樣寫不會有錯誤。<br>
因為B和1是已知的常數，編譯時就直接檢查相加沒超過char的範圍，就可以把值指派到變數。
{% highlight java linenos %}
char ch = 'B' + 1;
System.out.println(ch);
{% endhighlight %}
C

因為`[0]`是確定的變數，編譯時就直接檢查相加沒超過char的範圍，就可以把值指派到變數。
{% highlight java linenos %}
char[] arr1 = new char[10];
arr1[0] = 'A' + 1;
{% endhighlight %}
B

但若在迴圈中同樣的寫法會編譯錯誤，因為`arr1[i]`是不確定的變數。
{% highlight java linenos %}
for (int i = 0; i < arr1.length; i++) {
  arr1[i] = 'A' + i;
}
{% endhighlight %}

在迴圈中char計算的結果為int，int的大小是4byte，char是2byte，要把「計算公式」強制轉型。
{% highlight java linenos %}
char[] arr1 = new char[10];
for (int i = 0; i < arr1.length; i++) {
  arr1[i] = (char)('A' + i);
  System.out.println(arr1[i]);
}
{% endhighlight %}

## 指派陣列變數
- [Memory Model][1]

指派基本型態變數給基本型態變數，實際上是複製變數的值到新的變數，n2修改值，也不會影嚮n1的值，二個變數是獨立的。

![img]({{site.imgurl}}/java/assign_int1.png)

![img]({{site.imgurl}}/java/assign_int2.png)

![img]({{site.imgurl}}/java/assign_int3.png)

{% highlight java linenos %}
int n1 = 10;
// 複製n1的值10，到n2
int n2 = n1;
n2 = 80;
System.out.println(n1);
System.out.println(n2);
{% endhighlight %}
```
10
80
```

指派陣列變數給陣列變數，是把陣列的記憶體位址複製到另一個變數。

![img]({{site.imgurl}}/java/assign_arr1.png)

![img]({{site.imgurl}}/java/assign_arr2.png)

![img]({{site.imgurl}}/java/assign_arr3.png)

若修改陣列變數，會影嚮另一個陣列變數的值。

{% highlight java linenos %}
int[] arr1 = {1, 2, 3};
// arr1陣列的記憶體位址複製到arr2
int[] arr2 = arr1;
// 修改陣列變數，會影嚮另一個陣列變數的值
arr2[2] = 10;
System.out.println(Arrays.toString(arr1));
System.out.println(Arrays.toString(arr2));
{% endhighlight %}
```
[1, 2, 10]
[1, 2, 10]
```
## 陣列題目
### 找出陣列中最大值
1. 使用max變數，儲存陣列中最大的值。
2. 使用maxIndex變數，儲存陣列中最大的值的索引。
3. max與maxIndex初始化，為`arr[0]`與0;
4. 遍歷由i = 1開始，因為初始值都已經用0開始，沒必要自己跟自己比。
5. 遍歷陣列，若之後的值比max大，修改max與maxIndex，指到比較大的值與索引。

{% highlight java linenos %}
int[] arr1 = {10,5,49};
// 1. 使用max變數，儲存陣列中最大的值。
// 3. max初始化，為arr[0]
int max = arr1[0];
// 2. 使用maxIndex變數，儲存陣列中最大的值的索引。
// 3. maxIndex初始化0
int maxIndex = 0;
// 遍歷由i = 1開始，沒必要自己跟自己比
for (int i = 1; i < arr1.length; i++) {
  // 之後的值比max大
  if (arr1[i] > max) {
    // 指到比較大的值與索引
    max = arr1[i];
    maxIndex = i;
  }
}
System.out.println("max = " + max + ", maxIndex = " + maxIndex);
{% endhighlight %}
```
max = 49, maxIndex = 2
```

### 插入新數字
題目:有一個陣列，由小到大排序，要找到插入的位置，並把數字插入。<br>
1.先在陣列中找到比插入數字還大的數字，或與插入數字相等的數字，arr[i] \>= insertNumber<br>
2.index存放插入的位置。
{% highlight java linenos %}
int[] arr = {10, 12, 25, 33, 45};
int insertNumber = 25;
int index = -1;
for (int i = 0; i < arr.length; i++) {
  if (arr[i] >= insertNumber) {
    index = i;
    break;
  }
}
{% endhighlight %}

3.如果index為-1，代表陣列中所有數字都比插入的數字小，把索引設為陣列長度。
{% highlight java linenos %}
int[] arr = {10, 12, 25, 33, 45};
int insertNumber = 27;
int index = -1;
for (int i = 0; i < arr.length; i++) {
  if (arr[i] >= insertNumber) {
    index = i;
    break;
  }
}
if (index == -1) {
  index = arr.length;
}
{% endhighlight %}

4.建立一個新的陣列，大小為原本陣列大小 \+ 1，\+1是要放置新插入的數字。
{% highlight java linenos %}
int[] copyarr = new int[arr.length + 1];
{% endhighlight %}

5.複製arr陣列的數字到新陣列copyarr，「跳過index」，因為index要放插入的新數字。<br>
![img]({{site.imgurl}}/java/insert_arr.png)
<br>
{% highlight java linenos %}
for (int i = 0; i < arr.length; i++) {
  if (i >= index) {
    copyarr[i + 1] = arr[i];
  } else {
    copyarr[i] = arr[i];
  }
}
{% endhighlight %}

6.把新數字放在index的位置。
{% highlight java linenos %}
copyarr[index] = insertNumber;
{% endhighlight %}

7.arr的記憶體位址，改成copyarr的記憶體位址。
{% highlight java linenos %}
arr = copyarr;
{% endhighlight %}

完整程式碼
{% highlight java linenos %}
int[] arr = {10, 12, 25, 33, 45};
int insertNumber = 27;
int index = -1;
for (int i = 0; i < arr.length; i++) {
  if (arr[i] >= insertNumber) {
    index = i;
    break;
  }
}
if (index == -1) {
  index = arr.length;
}
int[] copyarr = new int[arr.length + 1];
for (int i = 0; i < arr.length; i++) {
  if (i >= index) {
    copyarr[i + 1] = arr[i];
  } else {
    copyarr[i] = arr[i];
  }
}
copyarr[index] = insertNumber;
arr = copyarr;
System.out.println(Arrays.toString(arr));
{% endhighlight %}

### 原地反轉陣列
原地的意思是不建立其它變數，不浪費其它記憶體空間。<br>
題目:把陣列123456，倒轉成654321，不使用其它變數儲存。<br>
1. 把陣列大小除2，代表要互換的次數。
2. 初始化2個變數，i指向陣列索引0，j指向陣列最後一個索引(陣列大小-1)。
3. i\+\+往陣列中間移動，j\-\-往陣列中間移動，i指向的值與j指向的值，兩兩交換。

{% highlight java linenos %}
int[] arr1 = {1, 2, 3, 4, 5, 6};
int times = arr1.length / 2;
for (int i = 0, j = arr1.length - 1; i < times; i++, j--) 
  int temp = arr1[i];
  arr1[i] = arr1[j];
  arr1[j] = temp;
}
System.out.println(Arrays.toString(arr1));
{% endhighlight %}
```
[6, 5, 4, 3, 2, 1]
```

[1]: {% link _pages/java/memory_model.md %}