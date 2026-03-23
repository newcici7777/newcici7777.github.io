---
title: init
date: 2026-03-22
keywords: Python, init
---
類別中的`__init__`方法，相當於Java與C++建構子。<br>

## 語法
```
def __init__(self, 參數1, 參數2, 參數3):
	程式碼
```
第一個參數為self，一定要存在。
參數可以0個到多個。

## 沒有參數的init方法
呼叫`類別名()`建立物件時，就會呼叫`__init__(self)`。<br>
以下程式碼呼叫Cat()，就會呼叫`__init__(self)`方法。<br>
{% highlight python linenos %}
class Cat:
    def __init__(self):
        print("init")


cat1 = Cat()
{% endhighlight %}
```
init
```

## 有參數的init方法
呼叫`類別名(參數)`，就會自動把參數代入`__init__(self, 參數)`方法。<br>
{% highlight python linenos %}
class Cat:
    def __init__(self, name, age):
        print(f"name = {name}, age = {age}")


cat1 = Cat("小白", 5)
cat2 = Cat("小黑", 10)
{% endhighlight %}
```
name = 小白, age = 5
name = 小黑, age = 10
```

## 自動建立init方法
`__init__(self)`方法若沒手動寫，也會自動建立。<br>

