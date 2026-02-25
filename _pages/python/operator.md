---
title: 算術運算子
date: 2026-02-25
keywords: Python, operator
---
加減乘，與其它程式語言一模一樣，不會有問題。
```
+ - *
```

## 除法 `/` 與 整除`//`

|運算子|描述|範例|傳回值型別|
|:----:|:----:|:----:|:----:|
|/|除|8/2 = 1.8|浮點數float|
|//|整除|8//2 = 1|整數int|
|%|餘數|-38 % -6 = -2|整數int|
|`**`|次方|2`**`4 = 16|整數int|

### 除法 回傳小數(float)
python的除法會回傳小數點，並且型別變成float。<br>
以下的程式碼，傳回1.8
{% highlight python linenos %}
var1 = 9 / 5
print("var1 type", type(var1), "var1 = ", var1)
{% endhighlight %}
```
var1 type <class 'float'> var1 =  1.8
```

若改為可以整除的算式，回傳的值仍是有小數點。
{% highlight python linenos %}
var1 = 8 / 2
print("var1 type", type(var1), "var1 = ", var1)
{% endhighlight %}
```
var1 type <class 'float'> var1 =  4.0
```

#### 與C++比較除法
{% highlight c++ linenos %}
int main() {
  int var1 = 9 / 5;
  cout << "var1 = " << var1 << endl;
  return 0;
}
{% endhighlight %}
```
var1 = 1
```

把`9`改成`9.0`浮點數，結果才會是有小數點的浮點數。
{% highlight c++ linenos %}
  double var1 = 9.0 / 5;
  cout << "var1 = " << var1 << endl;
{% endhighlight %}
```
var1 = 1.8
```

#### 與Java比較除法
Java的除法是去掉小數點，即便接收的計算結果的型別是double，仍是輸出整除的商數。
{% highlight java linenos %}
int i = 10 / 4;
System.out.println(i);
double d = 10 / 4;
System.out.println(d);
{% endhighlight %}
```
2
2.0
```

除非把除數或被除數改為浮點數，結果才會是有小數點的浮點數。
{% highlight java linenos %}
System.out.println(10 / 4.0);
{% endhighlight %}
```
2.5
```

### 整除`//` 回傳int

- [java floor][1]

```
9 / 5 = 1.8
```
整除`//`傳回小於或等於 1.8 的整數1，無條件去掉小數。

以下程式碼整除`//`傳回 <= 1.8 的「整數」，可以看出執行結果的差別嗎？
{% highlight python linenos %}
var1 = 9 / 5
var2 = 9 // 5
print("var1 type", type(var1), "var1 = ", var1)
print("var2 type", type(var2), "var2 = ", var2)
{% endhighlight %}
```
var1 type <class 'float'> var1 =  1.8
var2 type <class 'int'> var2 =  1
```

#### 負數整除 
負數整除，會傳回「小於」或「等於」的整數(包含負號)，無條件去掉小數。<br>
以下程式碼， `-9 / 2 = -4.5`，「小於」或「等於」-4.5的整數(包含負號)是-5。<br>
{% highlight python linenos %}
var1 = 9 // 2
var2 = -9 // 2
print("var1 type", type(var1), "var1 = ", var1)
print("var2 type", type(var2), "var2 = ", var2)
{% endhighlight %}
```
var1 type <class 'int'> var1 =  4
var2 type <class 'int'> var2 =  -5
```

## 餘數公式
C++, Java餘數公式，切記先乘除 後加減，以下沒有括號比較好記。
```
a % b    = a - a / b * b
```
加上括號
```
a % b    = a - ((a / b) * b)
-38 % -6 = -38 - ((-38 / -6) * -6) = -2
```

Python要改為整除
```
a % b    = a - ((a // b) * b)
-38 % -6 = -38 - ((-38 // -6) * -6) = -2
```

{% highlight python linenos %}
var2 = -38 % -6
print("var2 type", type(var2), "var2 = ", var2)
{% endhighlight %}
```
var2 type <class 'int'> var2 =  -2
```

## 次方`**`
```
2 ** 4 = 16
```
$$ 2^4 = 16 $$

2的4次方為16

{% highlight python linenos %}
var1 = 2 ** 4
print("var1 = ", var1)
{% endhighlight %}
```
var1 =  16
```


[1]: {% link _pages/java/math.md %}#floor
