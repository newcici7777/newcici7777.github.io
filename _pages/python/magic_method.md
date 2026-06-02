---
title: Magic method
date: 2026-06-02
keywords: Python, Magic method
---
## `__eq__()`比較是否相同
使用到 `==` 會呼叫 `__eq__()`

```
cat1 == cat2
```

以下程式碼，二個等號左邊 cat1 為 self ,等號右邊 cat2 為 other。
```
def __eq__(self, other):
def __eq__(本身物件, 要比較的物件):
def __eq__(cat1, cat2):
```

沒有覆寫`__eq__()`之前，比較的是記憶體位址。<br>

完整程式碼
{% highlight python linenos %}
class Cat:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __eq__(self, other):
        return self.name == other.name and self.age == other.age


cat1 = Cat("小咪",5)
cat2 = Cat("小咪",5)
print(f"cat1 == cat2 : {cat1 == cat2}")
{% endhighlight %}
```
cat1 == cat2 : True
```

## 斷行
使用圓括號斷行:
```
return (self.name == other.name and
        self.age == other.age)
```                
{% highlight python linenos %}
class Cat:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"name: {self.name}, age: {self.age}"

    def __eq__(self, other):
        return (self.name == other.name and
                self.age == other.age)


cat1 = Cat("小咪",5)
cat2 = Cat("小咪",5)
print(f"cat1 == cat2 : {cat1 == cat2}")
{% endhighlight %}

使用`\`斷行:
```
return self.name == other.name and \
        self.age == other.age
```
{% highlight python linenos %}
class Cat:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"name: {self.name}, age: {self.age}"

    def __eq__(self, other):
        return self.name == other.name and \
                self.age == other.age


cat1 = Cat("小咪",5)
cat2 = Cat("小咪",5)
print(f"cat1 == cat2 : {cat1 == cat2}")
{% endhighlight %}

## isinstance 判斷是否為某個類別
```
if isinstance(物件記憶體位址, 類別):
if isinstance(other, Cat):
```

{% highlight python linenos %}
def __eq__(self, other):
	# 判斷是否為相同類別
    if isinstance(other, Cat):
        return self.name == other.name and \
            self.age == other.age
    return False
{% endhighlight %}

## `__ne__()` 不等於
{% highlight python linenos %}
class Cat:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"name: {self.name}, age: {self.age}"

    def __eq__(self, other):
        if isinstance(other, Cat):
            return self.name == other.name and \
                self.age == other.age
        return False
    def __ne__(self, other):
        return not self.__eq__(other)

class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

cat = Cat("小咪", 5)
dog = Dog("小咪", 5)
print(f"cat != dog : {cat != dog}")
{% endhighlight %}
```
cat != dog : True
```

## 大於 小於

- `__lt__(self, other)` 小於 `x < y`
- `__le__(self, other)` 小於等於 `x <= y`
- `__gt__(self, other)` 大於 `x > y`
- `__ge__(self, other)` 大於等於 `x >= y`

{% highlight python linenos %}
class Cat:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"name: {self.name}, age: {self.age}"

    def __eq__(self, other):
        if isinstance(other, Cat):
            return self.name == other.name and \
                self.age == other.age
        return False

    def __ne__(self, other):
        return not self.__eq__(other)

    def __lt__(self, other):
        if isinstance(other, Cat):
            return self.age < other.age
        return False

    def __le__(self, other):
        if isinstance(other, Cat):
            return self.age <= other.age
        return False

    def __gt__(self, other):
        if isinstance(other, Cat):
            return self.age > other.age
        return False

    def __ge__(self, other):
        if isinstance(other, Cat):
            return self.age >= other.age
        return False


cat1 = Cat("小咪", 5)
cat2 = Cat("小咪", 10)
print(f"cat1 < cat2 : {cat1 < cat2}")
print(f"cat1 <= cat2 : {cat1 <= cat2}")
print(f"cat1 > cat2 : {cat1 > cat2}")
print(f"cat1 >= cat2 : {cat1 >= cat2}")
{% endhighlight %}
```
cat1 < cat2 : True
cat1 <= cat2 : True
cat1 > cat2 : False
cat1 >= cat2 : False
```
