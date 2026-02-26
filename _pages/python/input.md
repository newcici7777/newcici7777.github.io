---
title: input()
date: 2026-02-26
keywords: Python, input
---
語法
```
msg = input("提示訊息")
```

input傳回值是str，若輸入為數字，需要使用`int(input("提示訊息"))`,`float(input("提示訊息"))`函式轉型。

{% highlight python linenos %}
name = input("請輸入姓名:")
age = input("請輸入年齡:")
score = input("請輸入成績:")
print("name = ", name)
print("age = ", age)
print("score = ", score)
print("type of score = ", type(score))
print("type of age = ", type(age))
{% endhighlight %}
```
請輸入姓名:Mary
請輸入年齡:18
請輸入成績:88.5
name =  Mary
age =  18
score =  88.5
type of score =  <class 'str'>
type of age =  <class 'str'>
```

以下程式碼會錯，因為score是str，需強制轉型。
{% highlight python linenos %}
print("score + 10 = ", score + 10)
{% endhighlight %}
```
    print("score + 10 = ", score + 10)
                           ~~~~~~^~~~
TypeError: can only concatenate str (not "int") to str
```

強制轉型後的結果如下:
{% highlight python linenos %}
name = input("請輸入姓名:")
age = input("請輸入年齡:")
score = input("請輸入成績:")
print("name = ", name)
print("age = ", age)
print("score = ", score)
print("score + 10 = ", float(score) + 10)
{% endhighlight %}
```
請輸入姓名:Mary
請輸入年齡:age
請輸入成績:88.5
name =  Mary
age =  age
score =  88.5
score + 10 =  98.5
```