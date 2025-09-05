---
title: 二維陣列
date: 2024-06-18
keywords: c++, two dimensional arrays
---
Prerequisites:

- [指標陣列][1]

## 二維陣列Memory Layout
C與C++的二維陣列記憶體配置如下:<br>

arr指向`int* arr[3]`一維的「指標陣列」，陣列中每一格都是存放記憶體「位址」。<br>

指標陣列中每個元素的記憶體位址，指向的是大小為3的「一維陣列」，裡面儲存的是整數。<br>

![img]({{site.imgurl}}/c++/arr/arr2d1.png)<br>

arr陣列名，指向指標陣列的第1個元素的內容為0x2000。<br>

### 指標陣列
以下是印出一維指標陣列，存放的記憶體位址。<br>
{% highlight c++ linenos %}
int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
cout << "arr address =" << arr << endl;
cout << "arr[0] address =" <<  arr[0] << endl;
cout << "arr[1] address =" <<  arr[1] << endl;
cout << "arr[2] address =" <<  arr[2] << endl;
{% endhighlight %}
```
arr address =0x2000
arr[0] address =0x2000
arr[1] address =0x200C
arr[2] address =0x2018
```

### 陣列
指標陣列中每個元素的記憶體位址，指向的是大小為3的「一維陣列」，裡面儲存的是整數。<br>
要印出一維陣列的記憶體位址，前面要加上`&`，直接使用`arr[0][0]`印出的是存放的整數。<br>
{% highlight c++ linenos %}
int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
cout << "arr[0] addr=" << &arr[0][0] << ",val=" << arr[0][0] << endl;
cout << "arr[1] addr=" << &arr[1][0] << ",val=" << arr[1][0] << endl;
cout << "arr[2] addr=" << &arr[2][0] << ",val=" << arr[2][0] << endl;
{% endhighlight %}
```
arr[0] addr=0x2000,val=10
arr[1] addr=0x200C,val=40
arr[2] addr=0x2018,val=70
```

### 類型與大小
指標陣列，每一格存的類型是`int*`整數型態記憶體位址。<br>
![img]({{site.imgurl}}/c++/arr/arr2d2.png)<br>

陣列，每一格存的類型是整數。<br>
![img]({{site.imgurl}}/c++/arr/arr2d3.png)<br>

指標陣列的元素，指向的是int[3]整數陣列，陣列大小為3 \* 4byte = 12 byte。
![img]({{site.imgurl}}/c++/arr/arr2d4.png)<br>

{% highlight c++ linenos %}
int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
cout << sizeof(arr[0]) << endl;
cout << sizeof(arr[1]) << endl;
cout << sizeof(arr[2]) << endl;
{% endhighlight %}
```
12
12
12
```

## 一維陣列
陣列的記憶體位址是連續，`int[3][3]`會建立9個連續的記憶體空間。<br>

![img]({{site.imgurl}}/c++/arr/arr2d5.png)<br>

int是4byte，也就是每個記憶體的位移是4byte。<br>
`arr[0][0]`與`arr[0][1]`記憶體位址相差4byte。<br>

二維陣列宣告，以下二種方式都是正確，因為實際上是大小3 \* 3 \* 4byte = 36 byte 一維陣列。<br>
{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int arr[3][3] = {10, 20, 30, 40, 50, 60, 70, 80, 90};
{% endhighlight %}

陣列大小:
{% highlight c++ linenos %}
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  cout << sizeof(arr) << endl;
{% endhighlight %}
```
36
```

## arr與arr[0]與arr[0][0]記憶體位址都相同
注意arr與arr[0]前面是沒有`&`<br>
{% highlight c++ linenos %}
int main() {
  int arr[][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  cout << "arr address = " << arr << endl;
  cout << "arr[0] address = " << arr[0] << endl;
  cout << "arr[0][0] address = " << &arr[0][0] << endl;
  return 0;
}
{% endhighlight %}
```
arr address = 0x2000
arr[0] address = 0x2000
arr[0][0] address = 0x2000
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
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
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
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
  int column = sizeof(arr[0]) / sizeof(int);
  cout << "column = " << column << endl;
{% endhighlight %}

### 遍歷二維陣列
{% highlight c++ linenos %}
int main() {
  int arr[3][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
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
int arr[][3] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
{% endhighlight %}

注意！以下的寫法是錯誤，column欄的大小不能為空，這樣編譯器無法推導大小，以下是Java的寫法，不是C++的寫法。<br>
{% highlight c++ linenos %}
// 錯誤！
int arr[3][] = { {10, 20, 30}, {40, 50, 60}, {70, 80, 90} };
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

[1]: {% link _pages/c/array/arrayOfPointers.md %}