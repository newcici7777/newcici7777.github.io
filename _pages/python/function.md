---
title: function
date: 2026-03-03
keywords: Python, function
---
## 語法
有參數
```
def 函式名(參數1, 參數2...):
    程式碼
```

無參數
```
def 函式名():
    程式碼
```

有return
```
def 函式名(參數1, 參數2...):
    return 程式碼
```

無return
```
def 函式名(參數1, 參數2...):
    程式碼
```

{% highlight python linenos %}
def max(a, b):
    if a > b:
        return a
    else:
        return b


n = max(10, 5)
print(n)
{% endhighlight %}
```
10
```