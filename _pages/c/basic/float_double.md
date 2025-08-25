---
title: 浮點數
date: 2025-08-25
keywords: c++, float, double
---
有小數的就是浮點數。<br>

## 小數
.55<br>
55.0<br>
5.12e2，科學表示法 5.12 \* 10^2 = 5.12 \* 100 <br>
5.12E-2，科學表示法 5.12 \* 10^-2 = 5.12 / 100<br>
{% highlight c++ linenos %}
int main() {
  double d1 = .55;
  printf("d1 = %.2f \n", d1);
  double d2 = 55.0;
  printf("d2 = %.2f \n", d2);
  double d3 = 5.12e2;
  printf("d3 = %.2f \n", d3);
  double d4 = 5.12E-2;
  printf("d4 = %f \n", d4);
  return 0;
}
{% endhighlight %}
```
d1 = 0.55 
d2 = 55.00 
d3 = 512.00 
d4 = 0.051200 
```

## double與float
預設是double，若後面數字有加上f或F，就會是float。

以下是float。
{% highlight c++ linenos %}
float num = 10.55f;
{% endhighlight %}

以下是double，後面沒有f，後面沒有f預設就是double。
{% highlight c++ linenos %}
double num = 10.55;
{% endhighlight %}

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

## 格式化

|格式化|印出小數點|
|%f   | 6位 |
|%.2f | 2位 |
|%.15f| 15位|

格式化語法
```
float num = 10.55f;
printf("%f",num);
```

### float格式字串輸出
{% highlight c++ linenos %}
// 數字後面有f
float f1 = 3.8f;
// 預設%f印出6位小數點
printf("f1 = %f \n", f1);
// %.2f印出2位小數點
printf("f1 = %.2f \n", f1);
{% endhighlight %}
```
執行結果
f1 = 3.800000
f1 = 3.80
```

## 小數點
超過保留小數點，一律四捨五入。

|浮點數型態|占用Byte數量|保留小數點|
|:---:|:---:|:---:|
|float|4|6位小數點|
|double|8|15位小數點|

### float小數點
由執行結果可以發現，第6位小數已經被四捨五入，由5變成6。
{% highlight c++ linenos %}
int main() {
  // 15.6555555555預設是double
  // double指派給float
  // float只保留6位小數，最後一位四捨五入
  float num = 15.6555555555;
  // 格式化印出%f，預設是印出6位小數
  printf("%f", num);
  return 0;
}
{% endhighlight %}
```
15.655556
```

### double小數點
由執行結果可以發現，第15位小數已經被四捨五入，由5變成6。
{% highlight c++ linenos %}
int main() {
  // 15.1234567890123456預設是double
  // double指派給double
  double num = 15.1234567890123456;
  // 印出小數點15位
  printf("%.15f", num);
  return 0;
}
{% endhighlight %}
```
15.123456789012346
```

### 格式化不能用在整數，會印出0.00
int用%f的格式化會顯示0.000
{% highlight c++ linenos %}
int main() {
  // 整數
  int num = 20.856;
  // 印出小數點3位
  printf("%.3f", num);
  return 0;
}
{% endhighlight %}
```
0.000
```

### 格式化超出小數點範圍
正常格式化若在小數點範圍內，不足格式化的小數點都會補0。
{% highlight c++ linenos %}
int main() {
  // 15.12是double
  double num = 15.12;
  // 印出小數點5位
  printf("%.5f", num);
  return 0;
}
{% endhighlight %}
```
15.12000
```

#### 格式化超出float小數點範圍
小數點第6位以後都是在亂印。
{% highlight c++ linenos %}
int main() {
  // float超出小數點6位
  float num = 15.123456789f;
  // 印出小數點9位，超出float小數6位
  printf("%.9f", num);
  return 0;
}
{% endhighlight %}
```
15.123456955
```

#### 格式化超出double小數點範圍
小數點15位以後都是在亂印。
{% highlight c++ linenos %}
int main() {
  // double超出小數點15位
  double num = 15.123456789012345678;
  // 印出小數點18位，超出double15位
  printf("%.18f", num);
  return 0;
}
{% endhighlight %}
```
15.123456789012346135
```

## 有效數字
有效數字是指準確度的數字，例如float，有7位數字會是正確的，超過7位以上，第8位以後就會不正確。
有效數字範圍:包含小數點前面與後面的所有數字。

|浮點數型態|占用Byte數量|有效數字範圍|
|:---:|:---:|:---:|
|float|4|7位有效數字|
|double|8|15-16位有效數字|
|long double|不少於double|不少於double|

### float有效位數
{% highlight c++ linenos %}
float f1 = 123456789.123f;//float 有效數字範圍為7位
printf("f1 = %f \n", f1);
{% endhighlight %}
```
執行結果
f1 = 123456792.000000 
```
從執行結果可以發現第8位以後數字就不正確。

### double有效位數
{% highlight c++ linenos %}
double d1 = 123456789.123456789;
printf("d1 = %lf \n", d1);
{% endhighlight %}
```
執行結果
d1 = 123456789.123457  
```
從執行結果可以發現小數點第6位以後數字就不正確。

### long dobule有效位數
{% highlight c++ linenos %}
long double Ld1 = 123456789.123456789;
printf("Ld1 = %Lf \n", Ld1);
{% endhighlight %}
```
執行結果
Ld1 = 123456789.123457   
```
從執行結果可以發現跟double的結果相同，long double取決於系統是Linux/Mac/Windows，會呈現不同結果。

## 浮點數儲存方式
儲存方式，符號位 \+ 指數位 \+ 尾數位。<br>

## 自動轉型，大轉小
C與C++，可以把大的直接轉成小的，只是會出現警告，但java會出現編譯error。<br>

### 自動轉型，精度會丟失
把double自動轉int，小數直接去掉。
{% highlight c++ linenos %}
int main() {
  // 把double轉int，小數直接去掉
  int num = 20.856;
  // 印出整數
  printf("%d", num);
  return 0;
}
{% endhighlight %}
```
20
```
### double轉float
以下是double轉成float，15.6555555555是double。<br>
{% highlight c++ linenos %}
float num = 15.6555555555;
{% endhighlight %}

### 運算式中有浮點數，其它整數會自動轉型浮點數，不需要強制轉型。
{% highlight c++ linenos %}
cout << "8.0/5 = " << 8.0/5 << endl;
{% endhighlight %}
```
執行結果
8.0/5 = 1.6
```

## 強制轉型
使用強制轉型編譯器「不會出現警告」，C與C++是可以接受大的型別自動轉型小的型別。

語法
```
(資料型態)值、變數、常數、運算式
```

使用強制轉型，double強制轉型int，但小數點以後的數字直接去掉。
{% highlight c++ linenos %}
int i3 = (int)23.9999999;
cout << "i3 = " << i3 << endl;
{% endhighlight %}
```
執行結果
i3 = 23
```

運算式中，只要有一個是double，其它運算數字也會提升至double。<br>
以下結果，跟`8.0/5`是一樣的效果。<br>
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

### 強制轉型只對近距離的數字會轉型
以下程式碼只把3.5由double轉int，直接去掉小數點，並非轉型全部算式。<br>
變成3 \* 10。<br> 
{% highlight c++ linenos %}
int main() {
  int num = (int)3.5 * 10 + 5 * 1.5;
  printf("%d", num);
  return 0;
}
{% endhighlight %}
```
37
```
以下程式碼，把括號的內容()全轉成int
{% highlight c++ linenos %}
int main() {
  int num = (int)(3.5 * 10 + 5 * 1.5);
  printf("%d", num);
  return 0;
}
{% endhighlight %}
```
42
```
