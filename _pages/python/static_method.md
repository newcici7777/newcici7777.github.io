---
title: Static method
date: 2026-06-02
keywords: Python, Static method
---
## 語法
使用`@staticmethod`宣告靜態方法，參數不用有self。<br>
{% highlight python linenos %}
class Cat:
    def __init__(self, name):
        self.name = name

    @staticmethod
    def f():
        print(f"靜態方法")

# 類別名.靜態方法()
Cat.f()
# 物件.靜態方法()
Cat("小咪").f()
{% endhighlight %}

## 類別名，就是物件
Python的類別名本身就是物件。<br>
類別名可以呼叫成員方法，但要代入參數，參數為類別名，因為類別名本身就是物件。<br>

{% highlight python linenos %}
class Cat:
    name = "Default cat"
    def __init__(self, name):
        self.name = name
    def hi(self):
        print(f"hi() name = {self.name}")

# 類別名呼叫成員方法，要代入參數
Cat.hi(Cat)
# 物件呼叫成員方法，不用代入參數，self就是物件本身
Cat("小咪").hi()
{% endhighlight %}
```
hi() name = Default cat
hi() name = 小咪
```