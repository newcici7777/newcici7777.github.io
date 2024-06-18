---
title: 二維陣列
date: 2024-06-18
keywords: c++, two dimensional arrays
---

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


## 2維陣列與指標運用

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
    
    for(int i = 0; i < size; i++) {
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

## 二維陣列指標

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

上一個程式碼例子，二維陣列的指標型態是int[2][3]，無法轉成int*
```
int arr[2][3] = {0};
int* p = arr;
```
>> Cannot initialize a variable of type 'int *' with an lvalue of type 'int[2][3]'

必須轉型，才能轉成int*指標。

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
    for(int i = 0; i < len; i++) {
        for(int j = 0; j < 3; j++) {
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
## 三維陣列指標

三維陣列可以解釋為，一維陣列，裡面每一個元素又指向一維陣列，裡面每一個元素又指向一維陣列。

宣告方式

```
資料類型 (*指標變數)[有幾個一維陣列在裡面][一維陣列大小] = 三維陣列變數名;
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int (*p)[2][3], int len) {
    for(int i = 0; i < len; i++) {
        for(int j = 0; j < 2; j++) {
            for(int k = 0; k < 3; k++) {
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