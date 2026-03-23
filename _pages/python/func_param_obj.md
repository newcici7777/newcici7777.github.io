---
title: 函式參數為物件
date: 2026-03-23
keywords: Python, object as function parameter
---
將物件傳入函式，內容會改變。<br>
{% highlight python linenos %}
class Person:
    name = None
    age = None
    def __init__(self, name, age):
        self.name = name
        self.age = age

def change(person):
    person.name = "Mary"
    person.age = 10

p1 = Person("Bill", 20)
print(f"p1 name = {p1.name}, age = {p1.age}")
change(p1)
print(f"p1 name = {p1.name}, age = {p1.age}")
{% endhighlight %}
```
p1 name = Bill, age = 20
p1 name = Mary, age = 10
```

## 函式參數指向None
將物件傳入函式，並把它的記憶體位址變成None，不會影嚮外部的物件。<br>

<br>
![img]({{site.imgurl}}/python/param_obj_none1.png)<br>

<br>
![img]({{site.imgurl}}/python/param_obj_none2.png)<br>

<br>
![img]({{site.imgurl}}/python/param_obj_none3.png)<br>

<br>
![img]({{site.imgurl}}/python/param_obj_none4.png)<br>

完整程式碼
{% highlight python linenos %}
class Person:
    name = None
    age = None

    def __init__(self, name, age):
        self.name = name
        self.age = age


def change(person):
    # person 指向 None
    person = None

p1 = Person("Bill", 20)
print(f"p1 name = {p1.name}, age = {p1.age}")
change(p1)
print(f"p1 name = {p1.name}, age = {p1.age}")
{% endhighlight %}