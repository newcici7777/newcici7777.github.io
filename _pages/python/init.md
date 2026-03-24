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
使用`類別名()`建立物件時，就會呼叫`__init__(self)`。<br>
會隱藏的代入self參數，self參數就是物件的記憶體位址。<br>

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

## 自動產生init方法
`__init__(self)`方法若沒手動寫，也會自動產生。<br>

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

## 一個類別只有一個init方法，多個init只有最後一個生效
以下程式碼有二個init方法，但只有最後一個生效，試圖使用二個參數，會產生錯誤訊息，內容為:你給了3個參數(包含隱藏的self參數)，但實際上只要給二個參數(包含隱藏的self參數)。
{% highlight python linenos %}
class Person:
    name = None
    age = None

    def __init__(self, name, age):
        self.name = name
        self.age = age
        print("come here 1")

    def __init__(self, name):
        self.name = name
        print("come here 2")

p = Person("John", 20)
{% endhighlight %}
```
    p = Person("John", 20)
        ^^^^^^^^^^^^^^^^^^
TypeError: Person.__init__() takes 2 positional arguments but 3 were given
```

## 自動產生成員變數
以下程式碼沒有成員變數 name 與 age ，在 init 方法使用`self.成員變數 = 參數`，就會自動產生成員變數。<br>
{% highlight python linenos %}
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age


p2 = Person("Mary", 20)
print(f"p2 name = {p2.name} , age= {p2.age} ")
{% endhighlight %}
```
p2 name = Mary , age= 20
```

## init使用可變參數

- [可變參數][1]

使用可變參數就可以實現多個建構子。
{% highlight python linenos %}
class Person:
    name = None
    age = None

    def __init__(self, *args):
        if len(args) == 1:
            self.name = args[0]
        elif len(args) == 2:
            self.name = args[0]
            self.age = args[1]

p1 = Person("John")
print(f"p1 name = {p1.name} , age= {p1.age} ")

p2 = Person("Mary", 20)
print(f"p2 name = {p2.name} , age= {p2.age} ")
{% endhighlight %}
```
p1 name = John , age= None 
p2 name = Mary , age= 20 
```

## init不能return 傳回值
以下程式碼執行時會有錯誤。
{% highlight python linenos %}
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        return "abcd"

p2 = Person("Mary", 20)
print(f"p2 name = {p2.name} , age= {p2.age} ")
{% endhighlight %}
```
    p2 = Person("Mary", 20)
         ^^^^^^^^^^^^^^^^^^
TypeError: __init__() should return None, not 'str'
```

[1]: {% link _pages/python/function.md %}#可變參數


