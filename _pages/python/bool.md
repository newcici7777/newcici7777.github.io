---
title: bool
date: 2026-03-12
keywords: Python, bool
---
## bool的值
Python bool的值只有True與False。<br>

以下執行結果為False，類型為bool。<br>
{% highlight python linenos %}
n1 = 100
n2 = 200
result = n1 > n2
print(f"result = {result}, type = {type(result)}")
{% endhighlight %}
```
result = False, type = <class 'bool'>
```

## bool 比較 與 加減
bool可以跟其它類型(int, float, string)進行比較。<br>
進行比較的時候，True為1，False為0。<br>

以下程式碼，True在數字相加的時候變成1，False在數字相加的時候變成0。<br>
{% highlight python linenos %}
b1 = True
b2 = False
print(b1 + 10)
print(b2 + 10)
{% endhighlight %}
```
11
10
```

bool在比較時候變成1或0。<br>
{% highlight python linenos %}
b1 = True
b2 = False
if b1 == 1 :
    print("True")
if b2 == 0 :
    print("False")
{% endhighlight %}
```
True
False
```

## 非0為True 0為False
以下程式碼 `if 0:` 不會進入，因為0是False。<br>
其它都會進入 if 執行程式碼。<br>
{% highlight python linenos %}
if 0:
    print("0")
if 1:
    print("1")
if -1:
    print("-1")
if 1.0:
    print("1.0")
if "Hi":
    print("Hi")
{% endhighlight %}
```
1
-1
1.0
Hi
```

以下程式碼，n為0就不會進入 if 程式碼區塊。<br>
{% highlight python linenos %}
n = 0
if n:
    print("false")
n = -200.5
if n:
    print("True")
n = 'a'
if n:
    print("True")
{% endhighlight %}
```
True
True
```