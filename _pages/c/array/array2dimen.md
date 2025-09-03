---
title: 二維陣列
date: 2024-06-18
keywords: c++, two dimensional arrays
---
## 二維陣列Memory Layout
C與C++的二維陣列記憶體配置如下:<br>
在Stack空間，建立一個長長的陣列，假設是`int arr[3][3]`，會建立9個連續的記憶體空間。<br>
int是4byte，也就是每個記憶體的位移是4byte。<br>
`arr[0][0]`與`arr[0][1]`記憶體位址相差4byte。<br>

![img]({{site.imgurl}}/c++/arr/arr2d1.png)<br>

二維陣列宣告，以下二種方式都是正確，因為C++的Memory Layout確實是一個長長的一維陣列，只是編譯器會自動把一維轉成二維存取。
{% highlight c++ linenos %}
  int arr[3][3] = {{10, 20, 30}, {40, 50, 60}, {70, 80, 90}};
  int arr[3][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
{% endhighlight %}

## 二維陣列由一維陣列組成。
`arr[3][3]`二維陣列，分別由三個一維陣列所組成。分別為arr[0]、arr[1]、arr[2]，這三個一維陣列所組成。

![img]({{site.imgurl}}/c++/arr/arr2d2.png)<br>

arr[0]指向一維陣列第1個元素記憶體位址。<br>
arr[1]指向一維陣列第1個元素記憶體位址。<br>
arr[2]指向一維陣列第1個元素記憶體位址。<br>

arr[0]包含3個元素，arr[1]包含3個元素，arr[2]包含3個元素。

`arr[0]`，陣列名本身就是記憶體位址，印出陣列名就是印出陣列地址，陣列名[0]本身就是記憶體位址，印出陣列名[0]就是印出記憶體位址。<br>

arr[0]與arr[1]，中間差12byte。把arr[0]的記憶體位址0x2000 + 12 = 0x200C。<br>
{% highlight c++ linenos %}
  int arr[][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
  cout << "arr[0] address = " << arr[0] << endl;
  cout << "arr[1] address = " << arr[1] << endl;
  cout << "arr[2] address = " << arr[2] << endl;
{% endhighlight %}
```
arr[0] address = 0x2000
arr[1] address = 0x200C
arr[2] address = 0x2018
```

## 求出row列與column欄
```
arr[列][欄]
arr[row][column]
```

### 求出陣列有幾列row
2維陣列全部大小`sizeof(arr)` = int為4byte，陣列有9個元素，9 \* 4 byte = 36 byte<br>
`sizeof(arr[0])`取出第0列的大小，每一列row有3個元素 \* 4byte = 12byte <br>
sizeof(arr) / sizeof(arr[0]) = 36 / 12 = 3列<br>

{% highlight c++ linenos %}
  int arr[3][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
  int row = sizeof(arr) / sizeof(arr[0]);
  cout << "row = " << row << endl;
{% endhighlight %}
```
3
```

### 求出陣列有幾欄column
`sizeof(arr[0])`取出第0列的大小，每一列row有3個元素 \* 4byte = 12byte <br>
`sizeof(int)`為4byte <br>
sizeof(arr[0]) / sizeof(int) = 12 / 4byte = 3欄。<br>
{% highlight c++ linenos %}
  int arr[3][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
  int column = sizeof(arr[0]) / sizeof(int);
  cout << "column = " << column << endl;
{% endhighlight %}

### 遍歷二維陣列
{% highlight c++ linenos %}
int main() {
  //int arr[3][3] = {{10, 20, 30}, {40, 50, 60}, {70, 80, 90}};
  int arr[3][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
  int row = sizeof(arr) / sizeof(arr[0]);
  int column = sizeof(arr[0]) / sizeof(int);
  for (int i = 0; i < row; i++) {
    for (int j = 0; j < column; j++) {
      cout << arr[i][j] << " ";
    }
    cout << endl;
  }
  return 0;
}
{% endhighlight %}
```
10 20 30 
40 50 60 
70 80 90
```

## 二維陣列初始化注意事項
若二維陣列的值全部寫上，陣列row的大小宣告可以不寫。<br>
編譯器可從元素數量9 / 欄位大小3，推導出row列是3。<br>
{% highlight c++ linenos %}
int arr[][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
{% endhighlight %}

注意！以下的寫法是錯誤，column欄的大小不能為空，這樣編譯器無法推導大小，以下是Java的寫法，不是C++的寫法。<br>
{% highlight c++ linenos %}
// 錯誤！
int arr[3][] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
{% endhighlight %}

## 初始化
### 初始化方法1
{% highlight c++ linenos %}
  int arr[2][3] = { {1,2,3}, {4,5,6} };
  cout << "arr[0][0] = " << arr[0][0] << endl;
  cout << "arr[0][1] = " << arr[0][1] << endl;
  cout << "arr[0][2] = " << arr[0][2] << endl;
  cout << "arr[1][0] = " << arr[1][0] << endl;
  cout << "arr[1][1] = " << arr[1][1] << endl;
  cout << "arr[1][2] = " << arr[1][2] << endl;
{% endhighlight %}
```
arr[0][0] = 1
arr[0][1] = 2
arr[0][2] = 3
arr[1][0] = 4
arr[1][1] = 5
arr[1][2] = 6
```
### 初始化方法2
{% highlight c++ linenos %}
  int arr[2][3] = {1,2,3,4,5,6};
  cout << "arr[0][0] = " << arr[0][0] << endl;
  cout << "arr[0][1] = " << arr[0][1] << endl;
  cout << "arr[0][2] = " << arr[0][2] << endl;
  cout << "arr[1][0] = " << arr[1][0] << endl;
  cout << "arr[1][1] = " << arr[1][1] << endl;
  cout << "arr[1][2] = " << arr[1][2] << endl;
{% endhighlight %}

### 初始化方法3
第1個[]中不用寫長度，第2個[]中要寫長度。
{% highlight c++ linenos %}
  int arr[][3] = {1,2,3,4,5,6};
  cout << "arr[0][0] = " << arr[0][0] << endl;
  cout << "arr[0][1] = " << arr[0][1] << endl;
  cout << "arr[0][2] = " << arr[0][2] << endl;
  cout << "arr[1][0] = " << arr[1][0] << endl;
  cout << "arr[1][1] = " << arr[1][1] << endl;
  cout << "arr[1][2] = " << arr[1][2] << endl;
{% endhighlight %}

C11之後不用有等號

{% highlight c++ linenos %}
  int arr[][3]{1,2,3,4,5,6};
{% endhighlight %}

以上印出結果都是一樣。

## sizeof

sizeof在二維陣列的使用方式是取出陣列占記憶體全部的byte。

以下的例子二維陣列有6個元素，每個元素是整數4byte，所以陣列占的記憶體大小為6\*4 = 24

{% highlight c++ linenos %}
  int arr[][3] {1,2,3,4,5,6};
  cout << "arr size = " << sizeof(arr) << endl;
{% endhighlight %}

```
arr size = 24
```

## 二維陣列轉成一維陣列

- 2維陣列記憶體位址是連續
- 把陣列名指派給指標，是把陣列[0][0]的記憶體位址指派給指標
- 使用指標把連續的記憶體位址中的值印出來。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  //宣告陣列長度，初始化陣列全部元素為整數0
  int arr[2][3] = {0};
  
  //指派值到二維陣列
  arr[0][0] = 1;
  arr[0][1] = 2;
  arr[0][2] = 3;
  
  arr[1][0] = 4;
  arr[1][1] = 5;
  arr[1][2] = 6;
  
  //印出值
  cout << "arr[0][0] = " << arr[0][0] << endl;
  cout << "arr[0][1] = " << arr[0][1] << endl;
  cout << "arr[0][2] = " << arr[0][2] << endl;
  cout << "arr[1][0] = " << arr[1][0] << endl;
  cout << "arr[1][1] = " << arr[1][1] << endl;
  cout << "arr[1][2] = " << arr[1][2] << endl;
  
  //記憶體位址是連續
  cout << "arr[0][0]位址 = " << (long long)&arr[0][0] << endl;
  cout << "arr[0][1]位址 = " << (long long)&arr[0][1] << endl;
  cout << "arr[0][2]位址 = " << (long long)&arr[0][2] << endl;
  cout << "arr[1][0]位址 = " << (long long)&arr[1][0] << endl;
  cout << "arr[1][1]位址 = " << (long long)&arr[1][1] << endl;
  cout << "arr[1][2]位址 = " << (long long)&arr[1][2] << endl;
  
  //因為記憶體位址都是連續的，把二維陣列[0][0]記憶體位址指派給指標p
  int* p = (int*)arr;
  //取得arr變數記憶體大小
  int size = sizeof(arr) / sizeof(int);
  
  for (int i = 0; i < size; i++) {
    //使用指標運算印出每個元素的值
    cout << "*(p + " << i << ") = " << *(p+i) << ",";
    //使用索引方式印出值
    cout << " [" << i << "] = " << p[i] << endl;
  }
  return 0;
}
{% endhighlight %}

```
arr[0][0] = 1
arr[0][1] = 2
arr[0][2] = 3
arr[1][0] = 4
arr[1][1] = 5
arr[1][2] = 6
arr[0][0]位址 = 140702053823568
arr[0][1]位址 = 140702053823572
arr[0][2]位址 = 140702053823576
arr[1][0]位址 = 140702053823580
arr[1][1]位址 = 140702053823584
arr[1][2]位址 = 140702053823588
*(p + 0) = 1, [0] = 1
*(p + 1) = 2, [1] = 2
*(p + 2) = 3, [2] = 3
*(p + 3) = 4, [3] = 4
*(p + 4) = 5, [4] = 5
*(p + 5) = 6, [5] = 6
```
從以上結果可以發現，記憶體位址的差距為4byte，而且是連續的。

可以使用指標把二維陣列轉成一維指標陣列，印出值。

## 指標指向二維陣列

二維陣列可以解釋為，一維陣列，裡面每一個元素又指向一維陣列。

宣告方式

```
資料類型 (*指標變數)[二維陣列每個元素所指向的一維陣列大小] = 二維陣列變數名;
```

使用二維陣列指標一定要把\*指標變數用括號()包起來，因為有運算子優先順序的問題。

{% highlight c++ linenos %}
int main() {
  //[3]代表二維陣列每個元素是大小為3的一維陣列。
  int arr[2][3] = { {1,2,3},{4,5,6} };
  //[3]代表二維陣列每個元素是大小為3的一維陣列。
  int (*p)[3] = arr;
  return 0;
}
{% endhighlight %}

[上一個程式碼例子](#二維陣列轉成一維陣列) ，指標是指向二維陣列int[2][3]，無法轉成指向一維陣列的指標int*，所以要強制轉型成指向一維陣列的指標`(int*)二維陣列變數名`


以下程式碼原本指向一維陣列的指標，指向二維陣列會編譯錯誤。

```
//二維陣列
int arr[2][3] = {0};
//原本指向一維陣列的指標，直接指向二維陣列會編譯錯誤
int* p = arr;
```

> Cannot initialize a variable of type 'int *' with an lvalue of type 'int[2][3]'


二維陣列先轉成一維陣列，使用`(int*)`，再讓指標指向轉型成一維的陣列。

```
int arr[2][3] = {0};
int* p = (int*)arr;
```

## 二維陣列傳函式

方法有二種

- 指標方式 void func(int (*p)[3], int len);
- 陣列方式 void func(int p[][3], int len);

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int (*p)[3], int len) {
  for (int i = 0; i < len; i++) {
    for (int j = 0; j < 3; j++) {
      cout << "p[" << i << "][" << i << "] = " << p[i][j] << "\t";
    }
    cout << endl;
  }
}
int main() {
  //[3]代表每個元素是大小為3的一維陣列。
  int arr[2][3] = { {1,2,3},{4,5,6} };
  func(arr, 2);
  return 0;
}
{% endhighlight %}

```
p[0][0] = 1	p[0][0] = 2	p[0][0] = 3	
p[1][1] = 4	p[1][1] = 5	p[1][1] = 6
```
## 指標指向三維陣列

宣告方式

```
資料類型 (*指標變數)[陣列個數][元素個數] = 三維陣列變數名;
```

三維陣列傳函式的方法有二種

- 指標方式 

```
void func(int (*p)[2][3], int len);
```

- 陣列方式 

```
void func(int p[][2][3], int len);
```

### 印出陣列元素
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int (*p)[2][3], int len) {
  for (int i = 0; i < len; i++) {
    for (int j = 0; j < 2; j++) {
      for (int k = 0; k < 3; k++) {
        cout << "p[" << i << "][" << j << "][" << k << "] = " << p[i][j][k] << "\t";
      }
      cout << endl;
    }
    cout << endl;
  }
}
int main() {
  int arr[2][2][3] =
  {
    { {1,2,3}, {4,5,6} },
    { {7,8,9}, {10,11,12} }
  };
  func(arr, 2);
  return 0;
}
{% endhighlight %}

```
p[0][0][0] = 1	p[0][0][1] = 2	p[0][0][2] = 3	
p[0][1][0] = 4	p[0][1][1] = 5	p[0][1][2] = 6	

p[1][0][0] = 7	p[1][0][1] = 8	p[1][0][2] = 9	
p[1][1][0] = 10	p[1][1][1] = 11	p[1][1][2] = 12	

```

### 修改陣列元素
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
const int firstMax = 2;
const int secondMax = 2;
const int thirdMax = 3;
void modifyArr(int p[][secondMax][thirdMax], int len) {
  for (int i = 0; i < len; i++) {
    for (int j = 0; j < secondMax; j++) {
      for (int k = 0; k < thirdMax; k++) {
        //修改指標記憶體位址的值
        p[i][j][k] += 10;
      }
    }// end of j
  }// end of i
}
void printArr(int (*p)[secondMax][thirdMax], int len) {
  for (int i = 0; i < len; i++) {
    for (int j = 0; j < secondMax; j++) {
      for (int k = 0; k < thirdMax; k++) {
        cout << "p[" << i << "][" << j << "][" << k << "] = " << p[i][j][k] << "\t";
      }
      cout << endl;
    }// end of j
    cout << endl;
  }//end of  i
}
int main() {
  int arr[firstMax][secondMax][thirdMax] =
  {
    { {1,2,3}, {4,5,6} },
    { {7,8,9}, {10,11,12} }
  };
  modifyArr(arr, firstMax);
  printArr(arr, firstMax);
  return 0;
}

{% endhighlight %}

```
p[0][0][0] = 11	p[0][0][1] = 12	p[0][0][2] = 13	
p[0][1][0] = 14	p[0][1][1] = 15	p[0][1][2] = 16	

p[1][0][0] = 17	p[1][0][1] = 18	p[1][0][2] = 19	
p[1][1][0] = 20	p[1][1][1] = 21	p[1][1][2] = 22	
```