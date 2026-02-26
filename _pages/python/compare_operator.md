---
title: 比較運算子 邏輯運算子
date: 2026-02-25
keywords: Python, Comparison operator
---
比較運算子傳回的結果是True或False<br>

## 大於小於
相同數字「大於>」或「小於<」，結果都是False。
{% highlight python linenos %}
print("10 > 10 =", 10 > 10)
print("10 < 10 =", 10 < 10)
{% endhighlight %}
```
10 > 10 = False
10 < 10 = False
```

## 等於== is
Prerequisites:

- [id 記憶體位址][1]

== 等於是比較內容是否相等。<br>
is 比較記憶體位址是否相等。<br>
is not 比較記憶體位址是否「不相同」，「不相同」傳回True，「相同」傳回False。<br>

打開終端機
```
python3
```

以下判斷二個字串是否為相同記憶體位址。
```
 % python3
Python 3.14.3 (v3.14.3:323c59a5e34, Feb  3 2026, 11:41:37) [Clang 16.0.0 (clang-1600.0.26.6)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> a = "abc#"
>>> b = "abc#"
>>> a == b
True
>>> a is b
False
>>> a is not b
True
>>> exit
```

## and or not
### and
```
x and y
```
如果x為True，直接傳回y，因為x已經是True，那掌控結果的一定是y。<br>
如果x為False，直接傳回x，因為x已經是False，就不用再判斷y是什麼東西，直接把x傳回。<br>

Python跟C++一樣，0在python視為False，非0一律為True。<br>
{% highlight python linenos %}
x = 10
y = 5
print(f"{x} and {y} = ", x and y)

x = 0
y = 5
print(f"{x} and {y} = ", x and y)
{% endhighlight %}
```
10 and 5 =  5
0 and 5 =  0
```

### or
```
x or y
```
如果x為True，直接傳回x，因為兩者有一個為真，結果就是True。<br>
如果x為False，直接傳回y。<br>
{% highlight python linenos %}
x = 10
y = 5
print(f"{x} or {y} = ", x or y)

x = 0
y = 5
print(f"{x} or {y} = ", x or y)
{% endhighlight %}
```
10 or 5 =  10
0 or 5 =  5
```

### not
```
not x
```
如果x為True，傳回False。<br>
如果x為False，傳回True。<br>

{% highlight python linenos %}
x = 0
y = 5
print(f"not {x} = ", not x)
print(f"not {y} = ", not y)
print("not False = ", not False)
{% endhighlight %}
```
not 0 =  True
not 5 =  False
not False =  True
```


[1]: {% link _pages/python/id.md %}