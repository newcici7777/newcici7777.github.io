---
title: 整數
date: 2025-08-25
keywords: c++, int, long, short
---
## 整數資料型態
### 有正負符號
因為最左邊第1位bit作為正負符號，所以bit都少1個，比如short有2byte，2byte總共是16bit，最左邊作為正負符號，所以變成15bit，正數範圍$2 ^{15} - 1$，其中的-1是減掉0這個數字。

|整數型態|占用Byte數量|格式字串|範圍|範圍
|:---:|:---:|:---:|:---|:---|
|short|2|%hd|$-2 ^{15}$ 至 $2 ^{15} - 1$|-32,768 至 32,767|
|int  |4|%d |$-2 ^{31}$ 至 $2 ^{31} - 1$|-2,147,483,648 至 2,147,483,647|
|long |4|%ld|$-2 ^{31}$ 至 $2 ^{31} - 1$|-2,147,483,648 至 2,147,483,647|
|long long |8|%lld|$-2 ^{63}$ 至$2 ^{63} - 1$|-9,223,372,036,854,775,808 至 9,223,372,036,854,775,807|

### 只有正數
大小為正負符號的正數大小2倍，再加1，例:short正負符號，正數最大為32767，unsigned short最大為32767 \* 2 = 65534 \+ 1 

|整數型態|占用Byte數量|格式字串|範圍|範圍|
|:---:|:---:|:---:|:---|:---|
|unsinged short|2|%hu|$2 ^{16} - 1$|0 到 65,535|
|unsinged int  |4|%u |$2 ^{32} - 1$|0 到 4,294,967,295|
|unsinged long |4|%ud|$2 ^{32} - 1$|0 到 18,446,744,073,709,551,615|
|unsinged long long |8|%llu|2 ^{64} - 1$|0 到 18,446,744,073,709,551,615|

## 格式化
{% highlight c++ linenos %}
int main() {
  // 把double轉int，小數直接去掉
  int num = 20.856;
  // 印出整數
  printf("%d \n", num);
  // 印出long最大值
  long num1 = 2147483647;
  printf("%ld \n", num1);
  // 印出long long最小值
  long long num2 = -9223372036854775808;
  printf("%lld \n", num2);
  return 0;
}
{% endhighlight %}
```
20 
2147483647 
-9223372036854775808 
```
## 自動轉型
有正負符號由小到大排序如下:<br>
```
short → int → long → long long
```

正負符號與正數混合，由小到大排序如下:<br>
```
short → unsinged short → int → unsinged int → long → unsinged long → long long → unsinged long long
```

char在c++也是整數，占1byte。<br>
以下是自動轉型，小的可以自動轉成大的，以下由小到大排序，因為long與float比較二者大小的過程太複雜，我直接分成二部分，最終它們都能自動轉型成double。(不要糾結float指數是怎麼儲存)<br>
```
char → short → int → long → double
                    float → double
```

## 整數大小
{% highlight c++ linenos %}
cout << "size of int = " << sizeof(8) << endl;
{% endhighlight %}
```
執行結果
size of int = 4
```

## 由大轉小可以，資料會丟失，會出現警告
在C與C++，可以由大轉小，只是會出現精度會遺失的警告。<br>
但在Java，無法由大轉小，編譯器會出現error。<br>

以下程式碼把double轉成int，小數點直接去掉。
{% highlight c++ linenos %}
int main() {
  // 把double轉int，小數直接去掉
  int num = 20.856;
  // 印出小數點3位
  printf("%d", num);
  return 0;
}
{% endhighlight %}
```
20
```

###  整數相除，去掉小數點，只留正數，不會四捨五入
{% highlight c++ linenos %}
cout << "8/5 = " << 8/5 << endl;
{% endhighlight %}
```
執行結果
8/5 = 1
```

### 運算式中有低精準度與高精準度，低精準度會自動轉成高精準度，不需要強制轉型。
{% highlight c++ linenos %}
char c7 = 97;
int i1 = 25;
long long llong1 = 150000000000LL;
cout << "97 + 25 + 150000000000 = " << c7 + i1 + llong1 << endl;
{% endhighlight %}
```
執行結果
97 + 25 + 150000000000 = 150000000122
```

### 等號(=)左邊是高精準度，右邊是低精準度，低精準度會自動轉成高精準度，不需要強制轉型。
{% highlight c++ linenos %}
char c7 = 97;
int i2 = c7;
cout << "i2 = " << i2 << endl;
{% endhighlight %}
```
執行結果
i2 = 97
```
### 等號(=)左邊是整數，右邊是浮點數，浮點數小數點直接去掉，轉換成整數。
{% highlight c++ linenos %}
int i3 = 23.9999999;
cout << "i3 = " << i3 << endl;
{% endhighlight %}
```
執行結果
i3 = 23
```

### 等號(=)右邊超出範圍。
unsigned int最大的值是4294967295。

{% highlight c++ linenos %}
unsigned int ui1 = 4294967295u;
unsigned int ui2 = 4294967295u + 1;
unsigned int ui3 = 4294967295u + 2;
cout << "ui1 = " << ui1 << endl;
cout << "ui2 = " << ui2 << endl;
cout << "ui3 = " << ui3 << endl;
{% endhighlight %}
```
執行結果
ui1 = 4294967295
ui2 = 0
ui3 = 1
```
unsigned int最大的值是4294967295，若超出的二進位會被去掉。

|超出的二進位|二進位|十進位|輸出結果|
|---:|:---|:---:|:---:|
||11111111 11111111 11111111 11111111|4294967295|4294967295|
|~~00000001~~|00000000 00000000 00000000 00000000|4294967295 + 1|0|
|~~00000001~~|00000000 00000000 00000000 00000001|4294967295 + 2|1|

