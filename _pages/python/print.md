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

## 多行格式化
前面都是f開頭，字串尾部為斷行，後面不用 \+ 連結二個字串。
{% highlight python linenos %}
name = "Mary"
age = 20
score = 98.5
print(f"name: {name}, "
      f"age: {age}, "
      f"score: {score}")
{% endhighlight %}
```
name: Mary, age: 20, score: 98.5
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

## 分隔線
輸出20個分隔線。<br>
{% highlight python linenos %}
print("-" * 20)
{% endhighlight %}
```
--------------------
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

## %s %d %.1f
語法:
```
print(" %s, %d, %.f" % (參數1, 參數2, 參數3))
```
- %s 字串
- %d 整數
- %.f 浮點數，可以為.1f 小數點1位， .2f小數點2位 .f 無小數點

{% highlight python linenos %}
name = "Mary"
age = 20
score = 90.5
print("name = %s , age = %d , score = %.2f" % (name, age, score))
print("name = %s , age = %d , score = %.f" % (name, age, score))
{% endhighlight %}
```
name = Mary , age = 20 , score = 90.50
name = Mary , age = 20 , score = 90
```

## \{\} format
{% highlight python linenos %}
name = "Mary"
age = 20
score = 90.567
print("name = {} , age = {} , score = {}".format(name, age, score))
{% endhighlight %}
```
name = Mary , age = 20 , score = 90.567
```

使用`{0}` `{1}` `{2}`，告訴這個字串，0是對映第0個參數，1是對映第2個參數，2是對映第3個參數。<br>
{% highlight python linenos %}
name = "Mary"
age = 20
score = 90.567
print("name = {2} , age = {1} , score = {0}".format(score, age, name))
{% endhighlight %}
```
name = Mary , age = 20 , score = 90.567
```


