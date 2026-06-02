---
title: 覆寫str()方法
date: 2026-06-02
keywords: Python, str
---
## `__str__()`
print(物件) 或 str(物件)，都會呼叫父類別`__str__()`方法。<br>
格式如下
```
<__main__.物件 object at 16進位記憶體位址>
```
{% highlight python linenos %}
class Cat:
    def __init__(self, name):
        self.name = name

cat = Cat("小咪")
print(cat)
print(str(cat))
# 10進位整數 位址
print(f"memory id = {id(cat)}")
# 轉成16進位
print(f"hex memory id = {hex(id(cat))}")
{% endhighlight %}
```
<__main__.Cat object at 0x10a64b860>
<__main__.Cat object at 0x10a64b860>
memory id = 4469340256
hex memory id = 0x10a64b860
```

## `super.__str__()`
{% highlight python linenos %}
class Cat:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        print("呼叫父類別__str__()")
        # 呼叫父類別__str__()
        return super().__str__()
cat = Cat("小咪")
print(cat)
{% endhighlight %}
```
呼叫父類別__str__()
<__main__.Cat object at 0x10a8e3830>
```

## 覆寫`__str__()`
{% highlight python linenos %}
class Cat:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f"{self.name}"
cat = Cat("小咪")
print(cat)
{% endhighlight %}
```
小咪
```