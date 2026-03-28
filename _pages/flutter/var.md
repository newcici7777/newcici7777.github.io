---
title: 變數
date: 2026-03-28
keywords: flutter, dart, variable
---
變數命名的方式有三種
- var 變數名
- 類型 變數名
- dynamic 變數名

## var 變數名
以 var 宣告的變數，一旦指派值，若值是int，這個變數就會是 int 類型，之後不能再指派字串給這個變數。<br>
以下語法錯誤。<br>
{% highlight dart linenos %}
  var n1 = 10;
  n1 = "Hello";
{% endhighlight %}

## 類型 變數名
Python 的類型有以下幾種:
{% highlight dart linenos %}
  int n1 = 10;
  String str = "Hello";
  bool b1 = true;
  double d1 = 10.0;
  num n2 = 10;
  num n3 = 10.0;
{% endhighlight %}

## dynamic 變數
dynamic 就是可以指派不同類型的值，即便有值，也可以再次指派其它類型的值。<br>
{% highlight dart linenos %}
  dynamic n1 = 10;
  n1 = "Hello";
  n1 = true;
  n1 = [];
  n1 = {};
{% endhighlight %}

dynamic 在以下程式碼不會有任何語法錯誤，因為它不會知道變數類型是什麼，只有執行時，才會知道變數類型是什麼。<br>
{% highlight dart linenos %}
void main() {
  dynamic n1 = 10;
  n1.startsWith("1");
}
{% endhighlight %}
```
Unhandled exception:
NoSuchMethodError: Class 'int' has no instance method 'startsWith'.
```

## 變數類型
### String
{% highlight dart linenos %}
  String str = "Hello";
{% endhighlight %}

### num
數字類型有 int/double/num。<br>
num可以為整數、也可以為浮點數。<br>
n2 原本為 10 (整數)，也可以再指派為浮點數 15.5。<br>
{% highlight dart linenos %}
  num n2 = 10;
  num n3 = 10.0;
  n2 = 15.5;
  n3 = 20;
{% endhighlight %}

### bool
{% highlight dart linenos %}
  bool b1 = true;
  bool b2 = false;
  var b3 = true;
{% endhighlight %}

## print
輸出變數的方式
```
$變數 
${變數}
```

使用大括號，可以有運算式。
{% highlight dart linenos %}
 print("${10 + 21}");
{% endhighlight %}