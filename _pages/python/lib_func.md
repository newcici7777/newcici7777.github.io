---
title: 內建函式
date: 2026-03-13
keywords: Python, round, abs
---
內建函式就是呼叫函式時，不需要import 其它模組。<br>

## round
會自動四捨五入到指定位數。<br>

語法
```
round(數字,小數點位數)
```

{% highlight python linenos %}
print(round(55.19, 1))
{% endhighlight %}
```
55.2
```

## abs 絕對值
絕對值abs(參數)，參數若為負數，執行結果會變正數。
{% highlight python linenos %}
print(abs(-100))
print(abs(-11.5))
{% endhighlight %}
```
100
11.5
```