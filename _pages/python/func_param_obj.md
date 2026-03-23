---
title: 函式參數為物件
date: 2026-03-23
keywords: Python, object as function parameter
---
將物件傳入函式，可以修改物件的成員變數。<br>

在0x1000記憶體位址，建立Person物件，p1變數指到0x1000。<br>
![img]({{site.imgurl}}/python/param_obj1.png)<br>

1.在Stack建立change()函式空間。<br>
2.參數傳來的p1記憶體位址0x1000，複製給person區域變數。<br>
![img]({{site.imgurl}}/python/param_obj2.png)<br>

person的記憶體位址是0x1000，修改0x1000記憶體位址的name與age 屬性。<br>
![img]({{site.imgurl}}/python/param_obj3.png)<br>

離開change函式，change函式空間的person區域變數，記憶體位址被釋放。<br>
![img]({{site.imgurl}}/python/param_obj4.png)<br>

完整程式碼
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

在0x1000記憶體位址，建立Person物件，p1變數指到0x1000。<br>
![img]({{site.imgurl}}/python/param_obj_none1.png)<br>

1.在Stack建立change()函式空間。<br>
2.參數傳來的p1記憶體位址0x1000，複製給person區域變數。<br>
![img]({{site.imgurl}}/python/param_obj_none2.png)<br>

將person區域變數指派為None。<br>
![img]({{site.imgurl}}/python/param_obj_none3.png)<br>

離開change函式，change函式空間的person區域變數，記憶體位址被釋放。<br>
![img]({{site.imgurl}}/python/param_obj_none4.png)<br>

由以下執行結果可以發現，p1指向的物件沒有變成None。<br>

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
```
p1 name = Bill, age = 20
p1 name = Bill, age = 20
```