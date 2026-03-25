---
title: 繼承
date: 2026-03-24
keywords: Python, inherit
---
繼承使用圓括號(父類別)。<br>

語法:<br>
```
class 子類別(父類別):

class Child(Parent):
```
以上是 Child 類別繼承 Parent 類別，要繼承的類別寫在圓括號()中。<br>

## 繼承父類別的屬性
Child 子類別透過繼承，擁有父類別的屬性name，所以可以使用`self.name`。<br>
{% highlight python linenos %}
class Parent:
    name = "Mary"

class Child(Parent):
    def func1(self):
        print(f"name = {self.name}")

child = Child()
child.func1()
{% endhighlight %}
```
name = Mary
Hi
```

## 繼承父類別的方法
Child 子類別透過繼承，擁有父類別的hi()方法，所以可以使用`child.hi()`。<br>
{% highlight python linenos %}
class Parent:
    name = "Mary"
    def hi(self):
        print("Hi")

class Child(Parent):
    def func1(self):
        print(f"name = {self.name}")

child = Child()
child.hi()
{% endhighlight %}
```
Hi
```

## init super()
子類別繼承
{% highlight python linenos %}
class Parent:
    def __init__(self):
        print("Parent init")

class Child(Parent):
    def __init__(self):
        super().__init__()
        print("Child init")

child = Child()
{% endhighlight %}
```
Parent init
Child init
```

## 子類別無法讀取父類別私有屬性與方法
{% highlight python linenos %}
class Parent:
    __name = "Mary"
    def __hi(self):
        print("Hi")

class Child(Parent):
    def func1(self):
        print(f"name = {self.__name}")
        self.__hi()

child = Child()
child.func1()
{% endhighlight %}
```
    print(f"name = {self.__name}")
                    ^^^^^^^^^^^
AttributeError: 'Child' object has no attribute '_Child__name'
```

## 子類別init方法 有參數
父類別 Animal init方法有二個參數 height(身高) 與 weight(體重)<br>
子類別 Dog 有自己的屬性color。<br>
子類別 Dog 繼承 Animal ，子類別 init 方法，參數一定要有父類別 init 的參數 height, weight(參數位置可以隨意放) ，然後參放再放子類別的 color 屬性。<br>
在子類別 init 方法，使用`super().__init__(父類別屬性)` 呼叫父類別init方法，初始化父類別的屬性，super()代表父類別。<br>
若父類別與子類有同名get_info()方法，想使用父類別的get_info()方法，使用`super().get_info()`。<br>
{% highlight python linenos %}
class Animal:
    height = None
    weight = None

    def __init__(self, height, weight):
        self.height = height
        self.weight = weight

    def get_info(self):
        return f"height: {self.height}, weight: {self.weight}"


class Dog(Animal):
    color = None
    # 父類別參數位置可以隨便放
    def __init__(self, height, weight, color):
        super().__init__(height, weight)
        self.color = color

    def get_info(self):
        return f"{super().get_info()} , color: {self.color}"


dog1 = Dog(100, 5, "White")
print(dog1.get_info())
{% endhighlight %}
```
height: 100, weight: 5 , color: White
```

## 多繼承
Python 可以多繼承。<br>
語法:<br>
```
class 子類別(父類別1, 父類別2):
    程式碼
```

若父類別1與父類別2有相同名字的屬性，優先順序由左往右。<br>
{% highlight python linenos %}
class Parent1:
    name = "Mary"

    def hi(self):
        print("Parent1.hi()")


class Parent2:
    name = "John"

    def hi(self):
        print("Parent2.hi()")


class Child(Parent1, Parent2):
    def func1(self):
        print(f"name = {self.name}")
        self.hi()


child = Child()
child.func1()
{% endhighlight %}
```
name = Mary
Parent1.hi()
```

{% highlight python linenos %}
class Parent1:
    name = "Mary"

    def hi(self):
        print("Parent1.hi()")


class Parent2:
    name = "John"

    def hi(self):
        print("Parent2.hi()")


class Child(Parent2, Parent1):
    def func1(self):
        print(f"name = {self.name}")
        self.hi()


child = Child()
child.func1()
{% endhighlight %}
```
name = John
Parent2.hi()
```

## 呼叫父類別屬性與方法
語法
```
父類別名.屬性
父類別名.方法(self)
```

以下是繼承多個父類別，分別呼叫不同父類別的屬性與方法，注意！呼叫父類別.方法()，一定要代入參數self， self 代表子類別記憶體位址。<br>
{% highlight python linenos %}
class Parent1:
    name = "Mary"

    def hi(self):
        print("Parent1.hi()")


class Parent2:
    name = "John"

    def hi(self):
        print("Parent2.hi()")


class Child(Parent2, Parent1):
    def call_parent1(self):
        print(f"name = {Parent1.name}")
        Parent1.hi(self)
    def call_parent2(self):
        print(f"name = {Parent2.name}")
        Parent2.hi(self)

child = Child()
child.call_parent1()
child.call_parent2()
{% endhighlight %}
```
name = Mary
Parent1.hi()
name = John
Parent2.hi()
```

## super() 代表「直屬」父類別
child呼叫`super().name`，取得Father的name。<br>
child呼叫`super().hi()`，取得Father的hi()方法。<br>
{% highlight python linenos %}
class Grandpa:
    name = "Grandpa"

    def hi(self):
        print("Grandpa.hi()")


class Father(Grandpa):
    name = "Father"

    def hi(self):
        print("Father.hi()")


class Child(Father):
    def func1(self):
        print(f"name = {super().name}")
        super().hi()


child = Child()
child.func1()
{% endhighlight %}
```
name = Father
Father.hi()
```

### 若直屬父類別沒有屬性、方法，會找父類別的直屬父類
{% highlight python linenos %}
class Grandpa:
    name = "Grandpa"

    def hi(self):
        print("Grandpa.hi()")


class Father(Grandpa):
    age = 30


class Child(Father):
    def func1(self):
        print(f"name = {super().name}")
        super().hi()


child = Child()
child.func1()
{% endhighlight %}
```
name = Grandpa
Grandpa.hi()
```