---
title: print 格式化
date: 2026-03-03
keywords: Python, print
---
## 印出空格
以下程式碼print()中的逗號為「空格」。<br>
{% highlight python linenos %}
print("Hello", "World")
{% endhighlight %}
```
Hello World
```

也可搭配變數使用。
{% highlight python linenos %}
x = 10
print("x =", x)
{% endhighlight %}
```
x = 10
```
## 格式化
語法
```
print(f"{變數}")
```
使用f與花括號`{}`，可以直接輸出變數。<br>
{% highlight python linenos %}
x = 10
print(f"x = {x}")
{% endhighlight %}
```
x = 10
```
## print會換行
{% highlight python linenos %}
print("Hello")
print("World")
{% endhighlight %}
```
Hello
World
```
## `end=""` 不要換行
print後面加上`end=""`，就不會換行。
{% highlight python linenos %}
print("Hello ", end="")
print("World")
{% endhighlight %}
```
Hello World
```

## 空一行
語法
```
print()
print("")
```
以上二種方式都是空一行。<br>

{% highlight python linenos %}
print("Hello ")
print()
print("World")
{% endhighlight %}
```
Hello 

World
```
