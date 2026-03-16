---
title: lambda
date: 2026-03-16
keywords: Python, lambda
---
Prerequisites:

- [if else][1]
- [函式作為參數][2]

lambda為匿名函式，一次性使用，用完了就不會重覆使用這個函式。<br>

## 語法
用lambda來宣告，後面為參數，使用分號 : 來區分參數與邏輯。<br>
```
lambda 參數1, 參數2, 參數3 : 一行的邏輯
lambda x, y : x if x > y else y
```

lambda為函式的參數，並把1、2丟進lambda進行大小比較。<br>
{% highlight python linenos %}
def test(fun, a, b):
    print(f"type of fun = {type(fun)}")
    return fun(a, b)


print(f"max = {test(lambda x, y: x if x > y else y, 1, 2)}")
{% endhighlight %}
```
type of fun = <class 'function'>
max = 2
```

[1]: {% link _pages/python/if_else.md %}
[2]: {% link _pages/python/func_param.md %}