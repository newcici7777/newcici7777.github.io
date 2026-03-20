---
title: None
date: 2026-03-20
keywords: Python, None
---
## 成員變數是None
成員變數是None，輸出也是None。<br>
{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None

cat1 = Cat()
print(f"cat1 name: {cat1.name}, age: {cat1.age}, color: {cat1.color}")
{% endhighlight %}
```
cat1 name: None, age: None, color: None
```

## NoneType
物件設為None，輸出為None，類型為NoneType。<br>
{% highlight python linenos %}
cat1 = None
print(f"cat1 = {cat1}, type = {type(cat1)}")
{% endhighlight %}
```
cat1 = None, type = <class 'NoneType'>
```

## 對None取得屬性
以下會產生找不到屬性的Error。<br>
{% highlight python linenos %}
cat1 = None
print(f"cat1 name: {cat1.name}, age: {cat1.age}, color: {cat1.color}")
{% endhighlight %}
```
    print(f"cat1 name: {cat1.name}, age: {cat1.age}, color: {cat1.color}")
                        ^^^^^^^^^
AttributeError: 'NoneType' object has no attribute 'name'
```

## 空的物件 bool()是False
Python 所有型別都是物件，對空的物件進行bool(obj)強制轉型，結果都會是False。<br>
{% highlight python linenos %}
# bool False
print(f"bool(False) = {bool(False)}")
# 數字0
print(f"bool(0) = {bool(0)}")
# None
print(f"bool(None) = {bool(None)}")
# 空字串
print(f"bool(empty str) = " + str(bool("")))
# 空list
print(f"bool([]) = {bool([])}")
# 空tuple
print(f"bool(()) = {bool(())}")
# 空字典
print(f"bool(dict()) = {bool({})}")
# 空set
print(f"bool(set()) = {bool(set())}")
{% endhighlight %}
```
bool(False) = False
bool(0) = False
bool(None) = False
bool(empty str) = False
bool([]) = False
bool(()) = False
bool(dict()) = False
bool(set()) = False
```

對空的物件if判斷，都是False。<br>
{% highlight python linenos %}
num = 0
if num:
    print(num)
else:
    print("num = 0")

str1 = ""
if str1:
    print(str1)
else:
    print("empty string")

cat = None
if cat:
    print(cat)
else:
    print("empty cat")

list1 = []
if list1:
    print(list1)
else:
    print("empty list")

tuple1 = ()
if tuple1:
    print(tuple1)
else:
    print("empty tuple")

dict1 = {}
if dict1:
    print(dict1)
else:
    print("empty dict")

set1 = set()
if set1:
    print(set1)
else:
    print("empty set")
{% endhighlight %}
```
num = 0
empty string
empty cat
empty list
empty tuple
empty dict
empty set
```