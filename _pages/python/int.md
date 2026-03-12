---
title: 整數
date: 2026-03-11
keywords: Python, int
---
Python的數字類型為整數與浮點數。<br>

## 整數
整數可以為負數(例: - 1) 與 正整數(例: 100)。<br>

Python沒有long long(C++)、也沒有Big Decimal(Java)，不管多大的數字，都是整數。<br>

## 浮點數
浮點數可以為負小點數(例: - 1.5)與 正小數點 (例: 100.5)。<br>

## 變數要有初始值
以下程式碼會編譯錯誤，因為b沒有初始值。<br>
{% highlight python linenos %}
a = 1
b
{% endhighlight %}

## 變數沒有類型
- [type 類型][1]

Python的變數沒有類型，所謂的「類型」是指變數「指向的內容」型別。<br>

以下的程式碼，變數a是沒有類型的，a可以指向整數1，也可以指向字串Hi，變數指向的內容型別才是類型。<br>
使用type可以檢查變數指向的內容型別。<br>
{% highlight python linenos %}
a = 1
print(type(a))
a = "Hi"
print(type(a))
{% endhighlight %}
```
<class 'int'>
<class 'str'>
```

## 2進位 16進位 8進位

- 2進位 0b開頭
- 16進位 0x開頭
- 8進位 0o開頭
- 10進位 不用任何開頭

{% highlight python linenos %}
n2 = 0b10
print(f"2進位 n2: {n2}")
n16 = 0x10
print(f"16進位 n16: {n16}")
n8 = 0o10
print(f"8進位 n8: {n8}")
n10 = 10
print(f"10進位 n10: {n10}")
{% endhighlight %} 
```
2進位 n2: 2
16進位 n16: 16
8進位 n8: 8
10進位 n10: 10
```

## Python整數大小
### sys.getsizeof()
取得變數指向的內容記憶體大小。<br>
Python 的 sys.getsizeof() 函數返回的單位是位元組（Bytes）。<br>
使用前請先import sys。<br>
{% highlight python linenos %}
import sys
sys.getsizeof(n)
{% endhighlight %}

### 自動擴充記憶體大小
隨著內容的數字位數愈大，會自動擴充記憶體大小，每次擴充4byte。<br>

以下程式碼`**`代表次方，`2 ** 3 = 8` 2 的 3 次為 8 (`2 * 2 * 2 = 8`)<br>
下方程式碼記憶體大小間隔為4byte。<br>
{% highlight python linenos %}
import sys

n = 0
print(f"n size = {sys.getsizeof(n)}")
n = 2 ** 15
print(f"n size = {sys.getsizeof(n)}")
n = 2 ** 30
print(f"n size = {sys.getsizeof(n)}")
n = 2 ** 60
print(f"n size = {sys.getsizeof(n)}")
{% endhighlight %}
```
n size = 28
n size = 28
n size = 32
n size = 36
```

### 超過整數輸出最大的位數 ValueError
最大的位數為4300位，超過了就會輸出以下的錯誤，無法把超過4300位數的整數轉成字串輸出。<br>
```
Exceeds the limit (4300 digits) for integer string conversion
```

參考文件:<br>
<https://docs.python.org/zh-tw/3/library/stdtypes.html>

{% highlight python linenos %}
n = 9 ** 9999
print(f"n = {n}")
{% endhighlight %}
```
    print(f"n = {n} ")
                ^^^
ValueError: Exceeds the limit (4300 digits) for integer string conversion; use sys.set_int_max_str_digits() to increase the limit
```


[1]: {% link _pages/python/type.md %}