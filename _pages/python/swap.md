---
title: Swap
date: 2026-02-26
keywords: Python, Swap
---
Python 的交換二個變數的值，可透過以下方式交換。
{% highlight python linenos %}
a = 5
b = 100
print(f"交換前 a = {a}, b = {b}")
a, b = b, a
print(f"交換後 a = {a}, b = {b}")
{% endhighlight %}
```
交換前 a = 5, b = 100
交換後 a = 100, b = 5
```