---
title: if else
date: 2026-02-26
keywords: Python, if else
---
## 沒有三元運算
Python沒有`?:`三元運算，取而代之如下:
```
max = a if a > b else b
```
如果 a > b，傳回a。<br>
否則，傳回b。<br>

![img]({{site.imgurl}}/python/if_else1.png)<br>

{% highlight python linenos %}
a = 5
b = 100
max = a if a > b else b
print("max = ", max)
min = a if a < b else b
print("min = ", min)
{% endhighlight %}
```
max =  100
min =  5
```

## if
語法
```
if 條件:
	程式碼1
	程式碼2

	程式碼3
```
Python 的 if 沒有一對花括號`{}`包住，使用Tab或空格縮排，區分`程式碼區塊`。<br>
以下雖然有空一行，但前方的空格數量是一致，屬於同一個區塊。<br>
{% highlight python linenos %}
if 5 < 8 :
    print("hello")

    print("world")
print("ABC")
{% endhighlight %}
```
hello
world
ABC
```

即便空格很多，若前方空格數量是一致，屬於同一個區塊。<br>
{% highlight python linenos %}
if 5 < 8 :
            print("hello")
        
            print("world")
print("ABC")
{% endhighlight %}

### if 包 if
