---
title: 指標基本觀念
date: 2024-05-27
keywords: c++, pointer
---
## 變數記憶體位址
每個變數系統會分配"一塊"記憶體位址存放變數，位址通常是用16進制表示。

以下為宣告一個i的整數變數，初始值為55。
{% highlight c++ linenos %}
int i = 55;
{% endhighlight %}
int資料型態變數i，占用記憶體4 byte，變數i占的開始位址0x00000008至結束位址0x0000000B，總共占4Byte。<br>

### i變數memory layout

<table class="custom-table">
  <tr><th width="10%">記憶體開始與結束</th><th width="45%">記憶體位址</th><th width="45%">占用記憶體位址範圍</th></tr>
  <tr><td>結束</td><td>0xFFFFFFFF</td><td rowspan="5"></td></tr>
  <tr><td rowspan="15">&#8593;</td><td>......</td></tr>
  <tr><td>0x0000000E</td></tr>
  <tr><td>0x0000000D</td></tr>
  <tr><td>0x0000000C</td></tr>
  <tr><td>0x0000000B</td><td bgcolor="#FFDD55">i變數記憶體位址結束</td></tr>
  <tr><td>0x0000000A</td><td rowspan="2" bgcolor="#FFDD55">i = 55;</td></tr>
  <tr><td>0x00000009</td></tr>
  <tr><td>0x00000008</td><td bgcolor="#FFDD55">i變數記憶體位址開始</td></tr>
  <tr><td>0x00000007</td><td rowspan="8"></td></tr>
  <tr><td>0x00000006</td></tr>
  <tr><td>0x00000005</td></tr>
  <tr><td>0x00000004</td></tr>
  <tr><td>0x00000003</td></tr>
  <tr><td>0x00000002</td></tr>
  <tr><td>0x00000001</td></tr>
  <tr><td>開始</td><td>0x00000000</td></tr>
</table>

### 堆疊區域變數
Stack堆疊，以下的程式碼，a變數先push到Stack中，接著b變數再push到Stack中，然後b變數再pop出來，從0x7ff7bfeff460位址「開始」建立記憶體空間，double是8byte的記憶體空間大小，所以結束位址是0x7ff7bfeff467。<br>

接著a變數pop出來，從0x7ff7bfeff468位址「開始」建立記憶體空間，int是4byte的記憶體大小，所以結束位址是0x7ff7bfeff46B。<br>

- 區域變數先定義 → 比較高的位址
- 區域變數後定義 → 比較低的位址

![img]({{site.imgurl}}/pointer/stack_var1.png)

以下宣告a與b變數。
{% highlight c++ linenos %}
int main() {
  int a = 0;
  double b = 10.5;
  cout << "變數a位址 = " << &a << endl;
  cout << "變數b位址 = " << &b << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
變數a位址 = 0x7ff7bfeff468
變數b位址 = 0x7ff7bfeff460
```

|記憶體位址|占用記憶體範圍|
|:-------------|:--------:|
|0x7ff7bfeff46B|a結束|
|0x7ff7bfeff46A|↑|
|0x7ff7bfeff469|↑|
|0x7ff7bfeff468|a開始|
|0x7ff7bfeff467|b結束|
|0x7ff7bfeff466|↑|
|0x7ff7bfeff465|↑|
|0x7ff7bfeff464|↑|
|0x7ff7bfeff463|↑|
|0x7ff7bfeff462|↑|
|0x7ff7bfeff461|↑|
|0x7ff7bfeff460|b開始|

以下程式碼在函式中建立三個整數變數，並觀察三個變數的記憶體位址是由大至小遞減，並且記憶體位址都是以4byte(整數占4 byte記憶體空間)遞減，證明stack變數，記憶體位址是由大至小成長。
{% highlight c++ linenos %}
void funcMemoryLocation() {
  int var1 = 10;
  int var2 = 20;
  int var3 = 30;

  //印出記憶體位址
  cout << "var1 = " << (long long)&var1 << endl;  
  cout << "var2 = " << (long long)&var2 << endl;
  cout << "var3 = " << (long long)&var3 << endl;
}
{% endhighlight %}
```
執行結果
var1 = 140702053822444
var2 = 140702053822440
var3 = 140702053822436
```

|記憶體位址|占用記憶體範圍|
|:-------------|:--------:|
|140702053822447|var1結束|
|140702053822446|↑|
|140702053822445|↑|
|140702053822444|var1開始|
|140702053822443|var2結束|
|140702053822442|↑|
|140702053822441|↑|
|140702053822440|var2開始|
|140702053822439|var3結束|
|140702053822438|↑|
|140702053822437|↑|
|140702053822436|var3開始|

### 取位址運算子
使用`&`「取位址」運算子放在i變數前，可以取出i變數的記憶體位址，取出的位址是「開始」位址0x00000008。
{% highlight c++ linenos %}
&i
{% endhighlight %}

## 指標類型
基本型態(int,short,byte,long,long long, unsigned long, char, float, double ...)都有對映的指標類型(int * ,short * ,byte * ,long * ,long long * , unsigned long * , char * , float * , double * ...)。

指標是一個類型，有變數名，存放的值是記憶體位址。<br>

語法<br>
```
指標類型 變數名 = 記憶體位址
```

有一個`int * `指標類型，變數名是`p`，存放的值是i變數的記憶體位址，。
{% highlight c++ linenos %}
int i = 55;
int * p = &i;
{% endhighlight %}

## 指標類型與基本型態要對映
以下的程式碼，i變數是整數類型，p變數也要是整數類型的指標，因為p整數指標類型是儲存int整數變數的記憶體位址。
{% highlight c++ linenos %}
int i = 55;
int * p = &i;
{% endhighlight %}

以下會編譯錯誤，因為i是整數類型，而p是double浮點數指標類型，double指標類型無法儲存int變數的位址，二者類型無法匹配。
{% highlight c++ linenos %}
int i = 55;
double * p = &i;
{% endhighlight %}

以下會編譯錯誤，因為i是double浮點數類型，而p是整數指標類型，int整數指標類型無法儲存double變數的位址，二者類型無法匹配。
{% highlight c++ linenos %}
double i = 55;
int * p = &i;
{% endhighlight %}

以下是錯誤，不能直接把i指派給p，不能把int指派給int * 指標類型，二者類型不相容，p變數只能接受整數變數記憶體位址。
{% highlight java linenos %}
int i = 55;
int* p = i; 
{% endhighlight %}

### 指標大小
不管是「int * 」指標類型或其它指標類型，存放記憶體的大小一律為8 byte。<br>

指標變數的大小8byte，使用sizeof(指標變數)可以取得指標的大小，不管是什麼資料型態的指標，佔用記憶體的大小全部為8byte。
{% highlight c++ linenos %}
cout << sizeof(p1) << endl;
{% endhighlight %}
```
執行結果
8
```

### 指標占用記憶體位址範圍與存放的值
p變數存放的開始位址0x00000000至結束位址0x00000007，總共占8byte。<br>
`&p`變數的記憶體位址是0x00000000，取得的是「開始」位址。<br>
此時指標p存放的是，變數i的「開始」記憶體位址0x00000008。<br>

<table class="custom-table">
  <tr><th width="10%">記憶體開始與結束</th><th width="45%">記憶體位址</th><th width="45%">占用記憶體位址範圍與存放的值</th></tr>
  <tr><td>結束</td><td>0xFFFFFFFF</td><td rowspan="7"></td></tr>
  <tr><td rowspan="17">&#8593;</td><td>......</td></tr>
  <tr><td>0x00000010</td></tr>
  <tr><td>0x0000000F</td></tr>
  <tr><td>0x0000000E</td></tr>
  <tr><td>0x0000000D</td></tr>
  <tr><td>0x0000000C</td></tr>
  <tr><td>0x0000000B</td><td bgcolor="#FFDD55">i變數記憶體位址結束</td></tr>
  <tr><td>0x0000000A</td><td rowspan="2" bgcolor="#FFDD55">i = 55;</td></tr>
  <tr><td>0x00000009</td></tr>
  <tr><td>0x00000008</td><td bgcolor="#FFDD55">i變數記憶體位址開始</td></tr>
  <tr><td>0x00000007</td><td bgcolor="#77DDFF">p變數記憶體位址結束</td></tr>
  <tr><td>0x00000006</td><td rowspan="6" bgcolor="#77DDFF">p = 0x00000008</td></tr>
  <tr><td>0x00000005</td></tr>
  <tr><td>0x00000004</td></tr>
  <tr><td>0x00000003</td></tr>
  <tr><td>0x00000002</td></tr>
  <tr><td>0x00000001</td></tr>
  <tr><td>開始</td><td>0x00000000</td><td bgcolor="#77DDFF">p變數記憶體位址開始</td></tr>
</table>

### pointer memory layout
在堆疊Stack中，i變數的記憶體位址是0x00000008，i儲存的值是55，p變數的記憶體位址是0x00000000，p儲存的值是0x00000008。<br>

![img]({{site.imgurl}}/pointer/pointer_memory1.png)

### 指標類型的變數
使用指標類型的變數，實際上是取出變數的「值」。
{% highlight c++ linenos %}
cout << p << endl;
{% endhighlight %}

完整程式碼:<br>
{% highlight c++ linenos %}
int main() {
  int i = 55;  // 整數類型的變數i
  int* p = &i;  // 指標類型的變數p，儲存變數i的記憶體位址
  cout << "i的位址 = " << &i << endl;
  cout << "p變數的值 = " << p << endl;
  return 0;
}
{% endhighlight %}
```
i的位址 = 0x00000008
p變數的值 = 0x00000008
```
### 指標指向其它位址
```
變數名 = 其它記憶體位址
p = &j;
cout << "p變數的值 = " << p << endl;
```

再印出p，就會顯示p變數儲存的值，已改變成其它記憶體位址。

![img]({{site.imgurl}}/pointer/pointer_memory3.png)

完整程式碼:
{% highlight c++ linenos %}
int main() {
  int i = 55;  // 整數類型的變數i
  int* p = &i;  // 指標類型的變數p，儲存變數i的記憶體位址
  int j = 100;
  p = &j;
  cout << "i的位址 = " << &i << endl;
  cout << "j的位址 = " << &j << endl;
  cout << "p變數的值 = " << p << endl;
  return 0;
}
{% endhighlight %}
```
i的位址 = 0x0000000E
j的位址 = 0x0000000B
p變數的值 = 0x0000000B
```

其它範例:
{% highlight c++ linenos %}
int main() {
  int a = 100;
  int b = 200;
  int * p = &a;
  *p = 10;
  p = &b;
  *p = 20;
  cout << "a = " << a << endl;
  cout << "b = " << b << endl;
  return 0;
}
{% endhighlight %}
```
a = 10
b = 20
```

### 指標指向其它指標的值
```
指標類型 變數名 = 指標變數
int* p2 = p;
```
p是p變數的「值」。<br>
把p變數的「值」，儲存在p2的記憶體位址中，p2的值也跟p的值一樣。<br>
下圖中，p2變數也有自己的記憶體位址0x00000000E，但它儲存的是0x00000008。<br>

![img]({{site.imgurl}}/pointer/pointer_memory4.png)

{% highlight c++ linenos %}
int main() {
  int i = 55;
  int* p = &i;
  int* p2 = p;
  cout << "i 位址 = " << &i << endl;
  cout << "p 變數的值 = " << p << endl;
  cout << "p2 變數的值 = " << p2 << endl;
  return 0;
}
{% endhighlight %}
```
i 位址 = 0x00000008 
p 變數的值 = 0x00000008
p2 變數的值 = 0x00000008
```

### 指標類型的變數位址
使用`&`「取位址」運算子放在p變數前，可以取出p變數的記憶體位址。
{% highlight c++ linenos %}
cout << &p << endl;
{% endhighlight %}

完整程式碼:<br>
{% highlight c++ linenos %}
int main() {
  int i = 55;  // 整數類型的變數i
  int* p = &i;  // 指標類型的變數p，儲存變數i的記憶體位址
  cout << "i的位址 = " << &i << endl;
  cout << "p變數的值 = " << p << endl;
  cout << "p的位址 = " << &p << endl;
  return 0;
}
{% endhighlight %}
```
i的位址 = 0x00000008
p變數的值 = 0x00000008
p的位址 = 0x00000000
```
### \* 取值運算子
使用 \* 取值運算子，可以取得「記憶體位址」存放的值。<br>
以下程式碼印出p存放的值 = 0x00000008 記憶體位址<br>
{% highlight c++ linenos %}
cout << p << endl;
{% endhighlight %}

以下的程式碼等同於`*0x00000008`，取得0x00000008記憶體位址，存放的值。
{% highlight c++ linenos %}
cout << *p << endl;
{% endhighlight %}
```
55
```
顯示的結果為55。

![img]({{site.imgurl}}/pointer/pointer_memory2.png)

完整程式碼:<br>
{% highlight c++ linenos %}
int main() {
  int i = 55;  // 整數類型的變數i
  int* p = &i;  // 指標類型的變數p，儲存變數i的記憶體位址
  cout << "i的位址 = " << &i << endl;
  cout << "p變數的值 = " << p << endl;
  cout << "p的位址 = " << &p << endl;
  cout << "* p取得「記憶體位址」存放的值 = " << *p << endl;
  return 0;
}
{% endhighlight %}
```
i的位址 = 0x00000008
p變數的值 = 0x00000008
p的位址 = 0x00000000
* p取得「記憶體位址」存放的值 = 55
```

### \* 修改記憶體中的值
以下的方式，等同於`*0x00000008`，取得0x00000008記憶體位址，並修改0x00000008記憶體位址中的值，變成10。
{% highlight c++ linenos %}
*p = 10;
{% endhighlight %}

完整程式碼:<br>
{% highlight c++ linenos %}
int main() {
  int i = 55;  // 整數類型的變數i
  int* p = &i;  // 指標類型的變數p，儲存變數i的記憶體位址
  cout << "i的位址 = " << &i << endl;
  cout << "p變數的值 = " << p << endl;
  cout << "p的位址 = " << &p << endl;
  cout << "* p取得「記憶體位址」存放的值 = " << *p << endl;
  *p = 10;
  cout << "修改後，i = " << i << endl;
  cout << "修改後，*p = " << *p << endl;
  return 0;
}
{% endhighlight %}
```
i的位址 = 0x00000008
p變數的值 = 0x00000008
p的位址 = 0x00000000
* p取得「記憶體位址」存放的值 = 55
修改後，i = 10
修改後，*p = 10
```

### 程式碼
以下為宣告一個i1的整數變數，初始值為10。
{% highlight c++ linenos %}
int i1 = 10;
{% endhighlight %}

指標變數存放的內容是位址，以下為宣告一個p1的指標變數，使用&取得變數i1記憶體位址，p1變數儲存i1變數的記憶體位址。
{% highlight c++ linenos %}
int* p1 = &i1;
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  int* p1 = &i1;
  cout << "i1的位址=" << &i1 << endl;
  cout << "p1存放的位址 =" << p1 << endl;
  return 0;
}
{% endhighlight %}

第2行，宣告i1整型變數。

第3行，宣告p1整型指標變數，使用取位址運算子&，取出i1變數的記憶體開始位址，將位址放入p1指標。

第4行，印出i1的位址。

第5行，印出p1所存放的內容。

```
執行結果
i1的位址= 0x7ff7bfeff468
p1存放的位址 =0x7ff7bfeff468
```

從執行結果可以發現，印出的結果是一樣。

## 宣告指標的各種寫法
宣告指標可以把 * 放置在`變數前面`或`資料型態後面`或`資料型態與變數中間`，以下都是正確的。

### 一行宣告一個指標

{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  int *p1 = &i1;		//*在變數前面
  int* p2 = &i1;		//*在資料型態後面
  int * p3 = &i1;		//*在資料型態與變數中間
  cout << "i1的位址=" << &i1 << endl;
  cout << "p1存放的位址 =" << p1 << endl;
  cout << "p2存放的位址 =" << p2 << endl;
  cout << "p3存放的位址 =" << p3 << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
i1的位址=0x7ff7bfeff468
p1存放的位址 =0x7ff7bfeff468
p2存放的位址 =0x7ff7bfeff468
p3存放的位址 =0x7ff7bfeff468
```

### 一行同時宣告多個指標
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  char* ptr1,* ptr2,* ptr3;
  char *ptr4, *ptr5, *ptr6;
  return 0;
}
{% endhighlight %}

### 一行同時宣告指標與整數變數(容易搞混淆的宣告)

{% highlight c++ linenos %}
  int* p4, num3;
  //p4是指標
  p4 = &ii;
  //num3為int資料型態的變數
  num3 = 20;
  cout << "p4 位址=" << p4 << endl;
  cout << "p4 值=" << *p4 << endl;
  cout << "num3 值=" << num3 << endl;
{% endhighlight %}

```
執行結果
p4 位址=0x7ff7bfdff274
p4 值=10
num3 值=20
```

## 取值運算子*

取值運算子*是取得位址中存放的值，指標存放位址，指標也就等於位址。

`*指標`

以上語法為，取出位址中存放的值，而指標就是位址。

{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  int *p1 = &i1;    //將i1變數的位址存到p1指標變數
  cout << "取出p1位址的值 = " << *p1 << endl;	//取出位址存放的值。
  return 0;
}
{% endhighlight %}
```
執行結果
取出p1位址的值 = 10
```

## 修改位址存放的值*

`*指標 = 修改的內容`

把*放在指標變數前面，就可以在等於(=)後面放入要修改的內容。

{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  cout << "取出i1的值 = " << i1 << endl;
  int *p1 = &i1;
  cout << "取出p1位址的值 = " << *p1 << endl;
  *p1 = 20;
  cout << "取出p1位址的值 = " << *p1 << endl;
  cout << "取出i1的值 = " << i1 << endl;
  return 0;
}
{% endhighlight %}
第6行，把*放在指標變數p1前面，等於(=)後面放入要修改的內容20。
```
執行結果
取出i1的值 = 10
取出p1位址的值 = 10
取出p1位址的值 = 20
```

## 指標資料型態要與值一致

指標是存放位址，不能存放不是位址的值，會編譯錯誤。
{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  int *p1 = i1;
  return 0;
}
{% endhighlight %}

第2行，i1變數是整數10。

第3行，把10塞入指標，因為指標是存放位址，不能放10這個整數，產生編譯錯誤。

## 印出位址
### C的方式
使用`%p`。
{% highlight java linenos %}
int main() {
  int i = 55;
  printf("i的位址 = %p \n",&i);
  return 0;
}
{% endhighlight %}
```
i的位址 = 0x7ff7bfeff2e8 
```
### 十六進制
以下的方式印不出char變數的位址。

{% highlight c++ linenos %}
int main() {
  char c = 'a';
  cout << "變數c位址 = " << &c << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
變數c位址 = a
```

使用(void*)就可以印出16進制的位址

`(void*)指標變數`

{% highlight c++ linenos %}
int main() {
  char c = 'a';
  cout << "變數c位址 = " << (void*)&c << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
變數c位址 = 0x7ff7bfeff46b
```

### 十進制
`(long long)指標變數`

因為int只有4byte，位址轉成int整數會超出範圍，使用long long 8byte，就不會有超出數值範圍的問題。

### 程式碼範例如下:
{% highlight c++ linenos %}
int main() {
  int i1 = 10;
  int *p1 = &i1;
  cout << "16進制 &i1 = " << (void*)&i1 << endl;
  cout << "16進制 p1 = " << (void*)p1 << endl;
  cout << "10進制 &i1 = " << (long long)&i1 << endl;
  cout << "10進制 p1 = " << (long long)p1 << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
16進制 &i1 = 0x7ff7bfeff468
16進制 p1 = 0x7ff7bfeff468
10進制 &i1 = 140702053823592
10進制 p1 = 140702053823592
```

## 指標資料型態
指標的資料型態要與記憶體位址中的內容相符。

{% highlight c++ linenos %}
int i = 10;
int* p1 = &i;
double d = 15.5;
double* p2 = &d;
{% endhighlight %}

