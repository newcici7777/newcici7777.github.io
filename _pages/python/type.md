---
title: type() 變數型別
date: 2026-02-23
keywords: Python, type
---
python的變數可以一直更換型別，使用type()可以查看變數型別。
{% highlight python linenos %}
var1 = 10
print(type(var1))
var1 = "Hello"
print(type(var1))
var1 = 1.1
print(type(var1))
{% endhighlight %}
```
<class 'int'>
<class 'str'>
<class 'float'>
```