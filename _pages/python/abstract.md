---
title: 抽象方法
date: 2026-06-01
keywords: Python, ABC, abstractmethod 
---
## 語法
import
```
from abc import ABC, abstractmethod
```

抽象類別
1. 繼承ABC
2. 抽象方法上面加上`@abstractmethod`

{% highlight python linenos %}
from abc import ABC, abstractmethod
class Animal(ABC):
    def __init__(self, name):
        self.name = name
    @abstractmethod
    def eat(self):
        pass
# 繼承ABC，就是抽象類別，不能用抽象類別建立物件
animal = Animal("Animal")
{% endhighlight %}

{% highlight python linenos %}
from abc import ABC, abstractmethod
class Animal(ABC):
    def __init__(self, name):
        self.name = name
    @abstractmethod
    def eat(self):
        pass
    
class Tiger(Animal):
    def eat(self):
        print("Tiger is eating")

tiger1 = Tiger("Tiger1")
tiger1.eat()
{% endhighlight %}
```
Tiger is eating
```

## 抽象類別可以有普通方法
{% highlight python linenos %}
from abc import ABC, abstractmethod
class Animal(ABC):
    def __init__(self, name):
        self.name = name
    @abstractmethod
    def eat(self):
        pass
    def info(self):
        print(f"name: {self.name} ")

class Tiger(Animal):
    def eat(self):
        print("Tiger is eating")

tiger1 = Tiger("Tiger1")
tiger1.eat()
tiger1.info()
{% endhighlight %}
```
Tiger is eating
name: Tiger1 
```

## 繼承抽象類別必須實作所有方法
以下程式碼Animal 有 eat() 跟 sleep() 抽象方法，子類別Tiger都必須實作。
{% highlight python linenos %}
from abc import ABC, abstractmethod
class Animal(ABC):
    def __init__(self, name):
        self.name = name
    @abstractmethod
    def eat(self):
        pass
    @abstractmethod
    def sleep(self):
        pass
    def info(self):
        print(f"name: {self.name} ")

class Tiger(Animal):
    def eat(self):
        print("Tiger is eating")

tiger1 = Tiger("Tiger1")
tiger1.eat()
tiger1.info()
{% endhighlight %}
```
TypeError: Can't instantiate abstract class Tiger without an implementation for abstract method 'sleep'
```