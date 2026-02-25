---
title: 型別轉換int() float() str()
date: 2026-02-23
keywords: Python, type convert
---
## 自動轉型
### 整數 + 浮點數 = 浮點數
以下程式碼var1是整型，var2是浮點數，若低精度(整型)與高精度(浮點數)相加計算，所產生的計算結果會以高精度(浮點數)為主。
{% highlight python linenos %}
var1 = 10
print("var1 type", type(var1))
var2 = 1.2
print("var2 type", type(var2))
var3 = var1 + var2
print("var3 type", type(var3))
print("var3 =", var3)
{% endhighlight %}
```
var1 type <class 'int'>
var2 type <class 'float'>
var3 type <class 'float'>
var3 = 11.2
```

## 強制轉型
語法
```
int(x)
float(x)
str(x)
```
x可為變數，或常數。

常數範例
```
int(10.5)
float(10)
str(10)
```
上述的10就是常數。

### 字串 + 整數
以下程式碼會執行失敗，因為「字串」不能與整數相加，會產生型別錯誤。
{% highlight python linenos %}
var1 = 10
print("var1 type", type(var1))
var2 = "HELLO"
print("var2 type", type(var2))
var3 = var1 + var2
print("var3 type", type(var3))
print("var3 =", var3)
{% endhighlight %}
```
    var3 = var1 + var2
           ~~~~~^~~~~~
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

需將整數轉成字串，使用str()函式，此時的\+ 加號，是把二個字串相連在一起。
{% highlight kotlin linenos %}
var1 = 10
print("var1 type", type(var1))
var2 = "HELLO"
print("var2 type", type(var2))
var3 = str(var1) + var2
print("var3 type", type(var3))
print("var3 =", var3)
{% endhighlight %}
```
var1 type <class 'int'>
var2 type <class 'str'>
var3 type <class 'str'>
var3 = 10HELLO
```

### 轉型
將整數10轉成float，不會影嚮var1原本的型別，var1變數不管如何轉型，仍是整數型別。
{% highlight kotlin linenos %}
var1 = 10
var2 = float(var1)
print("var1 type", type(var1), "var1 = ", var1)
print("var2 type", type(var2), "var2 = ", var2)
var3 = str(var1)
print("var1 type", type(var1), "var1 = ", var1)
print("var3 type", type(var3), "var3 = ", var3)
{% endhighlight %}
```
var1 type <class 'int'> var1 =  10
var2 type <class 'float'> var2 =  10.0
var1 type <class 'int'> var1 =  10
var3 type <class 'str'> var3 =  10
```
