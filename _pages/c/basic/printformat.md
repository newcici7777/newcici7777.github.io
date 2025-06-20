---
title: 整數與浮點數
date: 2024-04-18
keywords: c++, printf
---
## 整數

### 整數資料型態

|整數型態|占用Byte數量|格式字串|取值範圍
|:---:|:---:|:---:|:---|
|int|4|%d|-2,147,483,648 至 2,147,483,647|
|unsinged int|4|%u|0 到 4,294,967,295|
|short|2|%hd|-32,768 至 32,767|
|unsinged short|2|%hu|0 到 65,535|
|long |4|%ld|-2,147,483,648 至 2,147,483,647|
|unsinged long |4|%ud  |0 到 18,446,744,073,709,551,615|
|long long |8|%lld|-9,223,372,036,854,775,808 至 9,223,372,036,854,775,807|
|unsinged long long |8|%llu|0 到 18,446,744,073,709,551,615|


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
long long llong1 = 150000000000;
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
unsigned int ui1 = 4294967295;
unsigned int ui2 = 4294967295 + 1;
unsigned int ui3 = 4294967295 + 2;
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

### 強制轉型

使用強制轉型編譯器不會出現警告。

語法
```
(資料型態)值、變數、常數、運算式
```


{% highlight c++ linenos %}
int i3 = (int)23.9999999;
cout << "i3 = " << i3 << endl;
{% endhighlight %}
```
執行結果
i3 = 23
```

{% highlight c++ linenos %}
cout << "(double)8/5 = " << (double)8/5 << endl;
{% endhighlight %}
```
執行結果
(double)8/5 = 1.6
```

### 括號的優先級別比較高

{% highlight c++ linenos %}
cout << "8/5 = " << (double)(8/5) << endl;
{% endhighlight %}
```
執行結果
8/5 = 1
```
以上程式碼先執行(8/5)，也就是整數相除會直接去掉小數點，變成1，1再轉成double資料型態。

### 整數資料型態都可以使用%d印出，只是印出的精準度不同。

## 浮點數

### 浮點數資料型態
有效數字是指準確度的數字，例如float，有7位數字會是正確的，超過7位以上，第8位以後就會不正確。
有效數字範圍:包含小數點前面與後面的所有數字。

|浮點數型態|占用Byte數量|格式字串|有效數字範圍|
|:---:|:---:|:---:|:---:|
|float|4|%f|7位有效數字|
|double|8|%lf|15-16位有效數字|
|long double|不少於double|%Lf|不少於double|

### 數字有小數點就視為double資料型態

{% highlight c++ linenos %}
cout << "size of int = " << sizeof(8) << endl;
cout << "size of double = " << sizeof(8.0) << endl;
{% endhighlight %}
```
執行結果
size of int = 4
size of double = 8
```

### 數字最後面有f視作float資料型態

{% highlight c++ linenos %}
cout << "size of float = " << sizeof(8.0f) << endl;
{% endhighlight %}
```
執行結果
size of float = 4
```

### float格式字串輸出

{% highlight c++ linenos %}
float f1 = 3.8f;
printf("f1 = %f \n", f1);
printf("f1 = %.2f \n", f1);
{% endhighlight %}

```
執行結果
f1 = 3.800000
f1 = 3.80
```

### double格式字串輸出

{% highlight c++ linenos %}
float f1 = 3.8;
double d1 = 3.8;
printf("f1 = %f , d1 = %f , d1 = %lf\n", f1, d1, d1);
printf("f1 = %.2f , d1 = %.2f , d1 = %.2lf\n", f1, d1, d1);
{% endhighlight %}

```
執行結果
f1 = 3.800000 , d1 = 3.800000 , d1 = 3.800000
f1 = 3.80 , d1 = 3.80 , d1 = 3.80
```
### 運算式中有浮點數，其它整數會自動轉型浮點數，不需要強制轉型。

{% highlight c++ linenos %}
cout << "8.0/5 = " << 8.0/5 << endl;
{% endhighlight %}
```
執行結果
8/5 = 1.6
```

### 有效位數

#### float有效位數
{% highlight c++ linenos %}
float f1 = 123456789.123f;//float 有效數字範圍為7位
printf("f1 = %f \n", f1);
{% endhighlight %}
```
執行結果
f1 = 123456792.000000 
```
從執行結果可以發現第8位以後數字就不正確。

#### double有效位數
{% highlight c++ linenos %}
double d1 = 123456789.123456789;
printf("d1 = %lf \n", d1);
{% endhighlight %}
```
執行結果
d1 = 123456789.123457  
```
從執行結果可以發現小數點第6位以後數字就不正確。

#### long dobule有效位數
{% highlight c++ linenos %}
long double Ld1 = 123456789.123456789;
printf("Ld1 = %Lf \n", Ld1);
{% endhighlight %}
```
執行結果
Ld1 = 123456789.123457   
```
從執行結果可以發現跟double的結果相同，long double取決於系統是Linux/Mac/Windows，會呈現不同結果。

