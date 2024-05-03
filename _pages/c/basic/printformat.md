---
title: 整數與浮點數
date: 2024-04-18
keywords: c++, printf
---
## 整數

### 整數資料型態

|整數型態|占用Byte數量|格式符|取值範圍
|:---:|:---:|:---:|:---|
|int|4|%d|-2,147,483,648 至 2,147,483,647|
|unsinged int|4|%u|0 到 4,294,967,295|
|short|2|%hd|-32,768 至 32,767|
|unsinged short|2|%hu|0 到 65,535|
|long |4|%ld|-2,147,483,648 至 2,147,483,647|
|unsinged long |4|%ud  |0 到 18,446,744,073,709,551,615|
|long long |8|%lld|-9,223,372,036,854,775,808 至 9,223,372,036,854,775,807|
|unsinged long long |8|%llu|0 到 18,446,744,073,709,551,615|


####  整數相除，去掉小數點，只留正數，不會四捨五入

{% highlight c++ linenos %}
cout << "8/5 = " << 8/5 << endl;
{% endhighlight %}
```
執行結果
8/5 = 1
```

#### 運算式中有低類型與高類型，低類型會自動轉成高類型，不需要手動轉換。

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

#### 等號(=)左邊是高類型，右邊是低類型，低類型會自動轉成高類型，不需要手動轉換。

{% highlight c++ linenos %}
char c7 = 97;
int i2 = c7;
cout << "i2 = " << i2 << endl;
{% endhighlight %}
```
執行結果
i2 = 97
```
#### 等號(=)左邊是整數，右邊是浮點數，浮點數小數點直接去掉，轉換成整數。

{% highlight c++ linenos %}
int i3 = 23.9999999;
cout << "i3 = " << i3 << endl;
{% endhighlight %}
```
執行結果
i3 = 23
```

#### 等號(=)右邊超出範圍。

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
unsigned int最大的值是4294967295，若超出的二進制會被去掉。

|超出的二進制|二進制|十進制|輸出結果|
|---:|:---|:---:|:---:|
||11111111 11111111 11111111 11111111|4294967295|4294967295|
|~~00000001~~|00000000 00000000 00000000 00000000|4294967295 + 1|0|
|~~00000001~~|00000000 00000000 00000000 00000001|4294967295 + 2|1|

#### 強制轉換

使用強制轉換編譯器不會出現警告。

語法
```
(強制轉換類型)值、變數、常數、運算式
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

#### 括號的優先級別比較高

{% highlight c++ linenos %}
cout << "8/5 = " << (double)(8/5) << endl;
{% endhighlight %}
```
執行結果
8/5 = 1
```
以上程式碼先執行(8/5)，也就是整數相除會直接去掉小數點，變成1，1再轉成double類型。


#### 整數資料型態都可以使用%d印出，只是印出的精準度不同。

## 浮點數

### 浮點數資料型態

|浮點數型態|占用Byte數量|格式符|
|:---:|:---:|:---:|
|float|4|%f|
|double|8|%lf|
|long double|8|%Lf|

### float格式化輸出

{% highlight c++ linenos %}
float f1 = 3.8;
printf("f1 = %f \n", f1);
printf("f1 = %.2f \n", f1);
{% endhighlight %}

```
執行結果
f1 = 3.800000
f1 = 3.80
```

### double格式化輸出

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
#### 運算式中有浮點數，其它整數會自動轉成浮點數，不需要手動轉換。

{% highlight c++ linenos %}
cout << "8.0/5 = " << 8/5 << endl;
{% endhighlight %}
```
執行結果
8/5 = 1.6
```
