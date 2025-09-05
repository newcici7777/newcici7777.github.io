---
title: 指標陣列
date: 2024-06-19
keywords: c++, point of arrays
---
指標陣列(point of arrays)，一維陣列裡面的值都是放記憶體位址，不是放整數也不是放字元。<br>

下圖有i1、i2、i3，三個整數變數。<br>
陣列存放的都是記憶體位址，陣列本身也有記憶體位址，陣列的記憶體位址由0x1000開始，因為存放的類型是指標，指標的記憶體大小為8byte，所以每一個索引的位址間距是8byte。<br>
![img]({{site.imgurl}}/c++/arr/arr_has_ptrs1.png)<br>

語法<br>
```
指標類型
int*   陣列名[陣列大小];
int*   ptr[3];
```

## 指標陣列
{% highlight c++ linenos %}
  int i1 = 10;
  int i2 = 20;
  int i3 = 30;
  //宣告指標陣列存放3個記憶體位址
  int* ptr[3] = {&i1, &i2, &i3};
{% endhighlight %}

## 取址與取值
方式有二種
- 指標方式

1. 第1次用取值運算子`*(ptr + i)`取出記憶體位址。<br>
2. 第2次再使用\*取值運算子`*(*(ptr + i))`，取出記憶體位址存放的內容。<br>

![img]({{site.imgurl}}/c++/arr/arr_has_ptrs2.png)<br>

{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  int i2 = 20;
  int i3 = 30;
  //宣告指標陣列存放3個記憶體位址
  int* ptr[3] = {&i1, &i2, &i3};
  for (int i = 0; i < 3; ++i) {
    printf("陣列位址為%p",ptr + i);
    printf(",存放的位址%p, 值為%d\n",*(ptr + i), *(*(ptr + i)));
  }
  return 0;
}
{% endhighlight %}
```
陣列位址為0x1000,存放的位址0x0001, 值為10
陣列位址為0x1008,存放的位址0x0005, 值為20
陣列位址為0x1020,存放的位址0x0009, 值為30
```

- 陣列方式

1. 使用&[索引]取出陣列記憶體位址。
2. 使用[索引]取出陣列存放的記憶體位址。
3. 使用取值運算子\*[索引]，取出存放的內容。

{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  int i2 = 20;
  int i3 = 30;
  //宣告指標陣列存放3個記憶體位址
  int* ptr[3] = {&i1, &i2, &i3};
  for (int i = 0; i < 3; ++i) {
    printf("陣列位址為%p", &ptr[i]);
    printf(",存放的位址%p, 值為%d\n", ptr[i], * ptr[i]);
  }
  return 0;
}
{% endhighlight %}
```
陣列位址為0x1000,存放的位址0x0001, 值為10
陣列位址為0x1008,存放的位址0x0005, 值為20
陣列位址為0x1020,存放的位址0x0009, 值為30
```
## 指標陣列存放多個字串常數位址
字串常數是放在RODATA(唯讀區)，與Stack區塊是不同的。
![img]({{site.imgurl}}/c++/arr/arr_has_ptrs3.png)<br>

注意！`%s`是逐一印出記憶體位址的字元，直到遇到`\0`，才會停止輸出，詳細內容請見[C字串][1]文章。
{% highlight c++ linenos %}
int main() {
  //宣告指標陣列存放3個記憶體位址
  char* ptr[3] = {"Hello", "World", "Cat"};
  for (int i = 0; i < 3; ++i) {
    printf("陣列位址%p", &ptr[i]);
    printf(",存放位址%p, 值為%s\n", ptr[i], ptr[i]);
  }
  return 0;
}
{% endhighlight %}
```
陣列位址為0x1000,存放的位址0x0001, 值為Hello
陣列位址為0x1008,存放的位址0x0005, 值為World
陣列位址為0x1020,存放的位址0x0009, 值為Cat
```

如果陣列初始化已經知道有多少值，就可以不用寫大小。
{% highlight c++ linenos %}
  //不用寫大小
  char* ptr[] = {"Hello", "World", "Cat"};
{% endhighlight %}

## 指標陣列存放多個字串常數位址2
使用指標陣列，存放多個字串常數的記憶體位址。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
const int DAYS = 7; //陣列裡的指標個數
int main() {
  //陣列存放7個指標，每個指標指向字串常數的第一個字元的位址
  char* arr_ptrs[DAYS] =
  {"Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"};//字串指標陣列
  
  //指標的固定大小是8byte，指標陣列存放7個指標，總共大小為8byte*7
  cout << "arr_ptrs size = " << sizeof(arr_ptrs) << endl;
  for (int i = 0; i < DAYS; i++)
    cout << arr_ptrs[i] << endl;
  return 0;
}
{% endhighlight %}
```
arr_ptrs size = 56
Sunday
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
```

[1]: {% link _pages/c/array/charArray.md %}
