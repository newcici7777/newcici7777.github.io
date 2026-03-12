---
title: range
date: 2026-02-28
keywords: Python, range
---
## range 不可變序列
range函式文件網址<https://docs.python.org/zh-tw/3.14/library/functions.html#func-range>

### 參數介紹
```
range(起始值start, 不包含結束值stop, 間隔step)
range(0, 10, 1)
```
以上產生0 - 9的數字 0, 1, 2, 3 ... 9

數學公式如下:
```
起始值start <= 要產生的數字 < 結束值stop
```

注意！不包含結束值10。

使用range，要用list(range)，把range強制轉型成list，才可以顯示數字。
```
list(range)
```

{% highlight python linenos %}
r = range(0, 10, 1)
print(list(r))
{% endhighlight %}
```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### 參數預設值
start 預設為0 <br>
step 預設為1 <br>

### range(10)
產生0 - 9的數字，因為start預設為0，step預設為1，若只有代入一個參數，這個參數是stop結束值，因為只有結束值沒有預設值，是必填，注意！不包含結束值。<br>
{% highlight python linenos %}
r = range(10)
print(list(r))
{% endhighlight %}
```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### range(1, 10)
產生1 - 9的數字，因為start修改為1，step預設為1，若只有代入二個參數，第1個參數是start = 1，第二個參數是stop結束值，注意！不包含結束值。<br>
{% highlight python linenos %}
r = range(1, 10)
print(list(r))
{% endhighlight %}
```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### range(1, 10, 2)
產生1, 3, 5, 7, 9的數字，不包含10。<br>
start修改為1，step修改為2。<br>

{% highlight python linenos %}
r = range(1, 10, 2)
print(list(r))
{% endhighlight %}
```
[1, 3, 5, 7, 9]
```
