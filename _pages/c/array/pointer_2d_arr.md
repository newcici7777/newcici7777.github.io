---
title: 指標指向2維陣列
date: 2024-06-18
keywords: c++, two dimensional arrays, pointer to array
---
Prerequisites:

- [指標陣列][1]

## 觀念
所謂的2維陣列，事實上是指向一維「指標陣列」，一維指標陣列的「元素」，再指向一維「整數陣列」。<br>

宣告方式:<br>
<span class="markline">圓括號</span>包住(\* 指標名)代表指標指向大小為3 \*4byte = 12 byte的「一維陣列」。<br>
```
int (* 指標名)[大小] = 陣列名;
int (* ptr)[3] = arr;
```

## 指標
指標有自己的記憶體位址，存放的記憶體位址是指向arr[0]的一維陣列。<br>
![img]({{site.imgurl}}/c++/arr/arr2d_p1.png)<br>
{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int (* ptr)[3] = arr;
  cout << "ptr addr = " << &ptr << endl;
  cout << "ptr value = " << ptr << endl;
{% endhighlight %}
```
ptr addr = 0x1000
ptr value = 0x2000
```

## 指標的移動
指標指向大小為3 \*4byte = 12 byte的「一維陣列」。<br>
所以指標的每一次移動是\+12byte，而不是\+4byte，它的移動單位是一維陣列大小12byte。<br>

![img]({{site.imgurl}}/c++/arr/arr2d_p2.png)<br>
![img]({{site.imgurl}}/c++/arr/arr2d_p3.png)<br>
![img]({{site.imgurl}}/c++/arr/arr2d_p4.png)<br>

{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int (* ptr)[3] = arr;
  cout << "ptr+0 addr=" << * (ptr + 0) << endl;
  cout << "ptr+1 addr=" << * (ptr + 1) << endl;
  cout << "ptr+2 addr=" << * (ptr + 2) << endl;
{% endhighlight %}
```
ptr+0 addr=0x2000
ptr+1 addr=0x200C
ptr+2 addr=0x2018
```
## 取值運算子\*
對指標陣列的元素使用「\*取值運算子」，可以取到指向一維陣列中的「記憶體位址」。<br>

下圖中，對`*(ptr + 2)`使用取值運算子，黃色區塊就是取到的位址。<br>

![img]({{site.imgurl}}/c++/arr/arr2d_p5.png)<br>

{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int (* ptr)[3] = arr;
  cout << "ptr+2 addr=" << * (ptr + 2) << endl;
{% endhighlight %}
```
ptr+2 addr=0x2018
```

再對記憶體位址，使用\*取值運算子，會得到存放的內容。
{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int (* ptr)[3] = arr;
  cout << "ptr+2 addr=" << * (ptr + 2) << endl;
  // 取出內容
  cout << "ptr+2 value=" << * ( * (ptr + 2)) << endl;
{% endhighlight %}
```
ptr+2 addr=0x2018
ptr+2 value=70
```

## 取值運算子\*與移動指標
先把位址使用取值運算子\*取出來，此時取出來的位址是`int*`整數指標類型，移動的單位是4byte，而不是12byte。<br>

再把取出的位址+1就會移動4byte到下一格的記憶體位址。<br>
![img]({{site.imgurl}}/c++/arr/arr2d_p6.png)<br>
{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int (* ptr)[3] = arr;
  cout << "addr=" << * (ptr + 2) + 1 << endl;
{% endhighlight %}
```
addr=0x201C
```

再對記憶體位址，使用\*取值運算子，會得到存放的內容。
{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int (* ptr)[3] = arr;
  cout << "value=" << * ( * (ptr + 2) + 1 ) << endl;
{% endhighlight %}
```
value=80
```

## 指標遍歷2維陣列
使用指標來遍歷2維陣列。
{% highlight c++ linenos %}
int main() {
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int (* ptr)[3] = arr;
  int rowsize = sizeof(arr) / sizeof(arr[0]);
  int colsize = sizeof(arr[0]) / sizeof(int);
  for (int i=0; i < rowsize;i++) {
    for(int j=0; j < colsize;j++) {
      cout << * ( * (ptr + i) + j) << " ";
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

另一種簡便的遍歷方式。
{% highlight c++ linenos %}
int main() {
  int arr[3][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
  int row = sizeof(arr) / sizeof(arr[0]);
  int column = sizeof(arr[0]) / sizeof(int);
  // 宣告指標，指向二維陣列
  // 注意！有括號包住指標(* ptr)，有圓括號包住代表指向二維陣列的指標
  int (* ptr)[3] = arr;
  for (int i = 0; i < row; i++) {
    for (int j = 0; j < column; j++) {
      // 用法跟2維陣列用法相同，但變數名改為ptr
      cout << ptr[i][j] << " ";
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

以下為舊的文章內容。

-----------------------------------------

## 指標指向二維陣列
指標指向二維陣列，英文是pointer to array。<br>
二維陣列可以解釋為，一維陣列，裡面每一個元素又指向一維陣列。<br>

宣告方式
```
資料類型 (*指標變數)[二維陣列每個元素所指向的一維陣列大小] = 二維陣列變數名;
```
使用二維陣列指標一定要把\*指標變數用括號()包起來，因為有運算子優先順序的問題。<br>
{% highlight c++ linenos %}
int main() {
  //[3]代表二維陣列每個元素是大小為3的一維陣列。
  int arr[2][3] = { {1,2,3},{4,5,6} };
  //[3]代表二維陣列每個元素是大小為3的一維陣列。
  int (* p)[3] = arr;
  return 0;
}
{% endhighlight %}

## 二維陣列傳函式
方法有二種<br>
- 指標方式 void func(int (*p)[3], int len);
- 陣列方式 void func(int p[][3], int len);

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int (* p)[3], int len) {
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

## 指標轉型
以下程式碼原本指向一維陣列的指標，指向二維陣列會編譯錯誤。<br>
```
//二維陣列
int arr[2][3] = {0};
//原本指向一維陣列的指標，直接指向二維陣列會編譯錯誤
int* p = arr;
```
> Cannot initialize a variable of type 'int \*' with an lvalue of type 'int[2][3]'


二維陣列先轉成一維陣列，使用`(int*)`，再讓指標指向轉型成一維的陣列。
```
int arr[2][3] = {0};
int* p = (int*)arr;
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

## 指標指向三維陣列
宣告方式
```
資料類型 (*指標變數)[陣列個數][元素個數] = 三維陣列變數名;
```

三維陣列傳函式的方法有二種<br>
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
void func(int (* p)[2][3], int len) {
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

[1]: {% link _pages/c/array/arrayOfPointers.md %}#取址與取值