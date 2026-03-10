---
title: import 其它檔案的函式
date: 2026-03-10
keywords: Python, function import
---
test2.py 內容如下:
{% highlight python linenos %}
def add(x, y):
    return x + y
{% endhighlight %}

test1.py 內容如下
{% highlight python linenos %}
import test2

result = test2.add(10, 20)
print(result)
{% endhighlight %}
```
30
```