---
title: class 類別
date: 2026-03-20
keywords: Python, class
---
## 建立類別
在類別中的「變數」，稱為「屬性」或「成員變數」。<br>

語法:
```
class 類別名:
	變數
	函式
```

{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None
{% endhighlight %}

None是沒有值。<br>
None介紹:<https://docs.python.org/zh-tw/3.12/library/constants.html#None><br>
None相當於Java null, C++ nullptr 或是 0<br>

## 建立物件
```
物件變數 = 類別()
```

{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None

cat1 = Cat()
cat1.name = "小白"
cat1.age = 1
cat1.color = "while"
print(f"cat1 name: {cat1.name}, age: {cat1.age}, color: {cat1.color}")
cat2 = Cat()
cat2.name = "小黑"
cat2.age = 5
cat2.color = "black"
print(f"cat2 name: {cat2.name}, age: {cat2.age}, color: {cat2.color}")
{% endhighlight %}
```
cat1 name: 小白, age: 1, color: while
cat2 name: 小黑, age: 2, color: black
```

## 方法
在類別中的「函式」，稱為「方法」。<br>

- 方法的第一個參數要寫上self。
- 呼叫方法時，不用傳第一個參數self。
- 呼叫方法時，底層會偷偷把「呼叫方法的物件」傳入第一個參數self。
- 在方法內部，需要使用`self.成員變數`，才能存取成員變數。

{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None
    # 方法的第一個參數要寫上self
    def getInfo(self):
        # 在方法內部，需要使用self.成員變數，才能存取成員變數
        print(f"name = {self.name}, age = {self.age}, color = {self.color}")

cat1 = Cat()
cat1.name = "小白"
cat1.age = 1
cat1.color = "while"
# 呼叫方法時，不用傳第一個參數self。
# 底層會偷偷把「呼叫方法的物件」傳入第一個參數self
cat1.getInfo()

cat2 = Cat()
cat2.name = "小黑"
cat2.age = 5
cat2.color = "black"
cat2.getInfo()
{% endhighlight %}
```
name = 小白, age = 1, color = while
name = 小黑, age = 5, color = black
```

### 動態增加屬性
以下程式碼，只有針對cat1才新增owner屬性，不是每個Cat物件都有owner，只特定針對cat1物件才有。<br>
{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None
    def getInfo(self):
        print(f"name = {self.name}, age = {self.age}, color = {self.color}")

cat1 = Cat()
cat1.name = "小白"
cat1.age = 1
cat1.color = "while"
cat1.getInfo()
# 動態增加owner屬性
cat1.owner = "Mary"
print("cat1 owner =", cat1.owner)
{% endhighlight %}
```
name = 小白, age = 1, color = while
cat1 owner = Mary
```

cat2試圖取出owner屬性，會產生AttributeError
{% highlight python linenos %}
cat2 = Cat()
cat2.name = "小黑"
cat2.age = 5
cat2.color = "black"
cat2.getInfo()
print("cat2 owner =", cat2.owner)
{% endhighlight %}
```
    print("cat2 owner =", cat2.owner)
                          ^^^^^^^^^^
AttributeError: 'Cat' object has no attribute 'owner'
name = 小黑, age = 5, color = black
```
### 動態增加方法
{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None
    def getInfo(self):
        print(f"name = {self.name}, age = {self.age}, color = {self.color}")

# jump()函式 準備給cat1動態增加方法
def jump():
    print("jump")

cat1 = Cat()
cat1.name = "小白"
cat1.age = 1
cat1.color = "while"
cat1.getInfo()
cat1.owner = "Mary"
print("cat1 owner =", cat1.owner)
# 動態增加method1()方法，指派jump給method1，注意，不用有圓括號在函式名後面
cat1.method1 = jump
# 呼叫動態增加method1方法
cat1.method1()
{% endhighlight %}
```
name = 小白, age = 1, color = while
cat1 owner = Mary
jump
```

method1()方法只針對cat1物件才有，cat2沒有method1()方法。<br>
{% highlight python linenos %}
cat2 = Cat()
cat2.method1()
{% endhighlight %}
```
    cat2.method1()
    ^^^^^^^^^^^^
AttributeError: 'Cat' object has no attribute 'method1'
```

### 動態增加方法類型
動態增加方法的類型為 function ，Cat 類別中的 getInfo() 方法的類型為 method <br>
{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None
    def getInfo(self):
        print(f"name = {self.name}, age = {self.age}, color = {self.color}")

def jump():
    print("jump")

cat1 = Cat()
cat1.name = "小白"
cat1.age = 1
cat1.color = "while"
cat1.method1 = jump
print("type(jump) = ", type(jump))
print("type(cat1.method1) = ", type(cat1.method1))
print("type(cat1.getInfo) = ", type(cat1.getInfo))
{% endhighlight %}
```
type(jump) =  <class 'function'>
type(cat1.method1) =  <class 'function'>
type(cat1.getInfo) =  <class 'method'>
```

### 方法參數
getInfo()方法參數 name ，是區域變數。<br>
類別中的name是成員變數，二者是不同。<br>
方法參數 name 與 成員變數 name 相同名字，但方法參數 name 不會「自動」覆蓋成員變數 name。
{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None

    def getInfo(self, name):
        print("name = ", name)
        print("self.name = ", self.name)

cat1 = Cat()
cat1.name = "小白"
cat1.age = 1
cat1.color = "while"
cat1.getInfo("小黃")
{% endhighlight %}
```
name =  小黃
self.name =  小白
```

需要使用`self.成員變數 = 方法參數`，才會修改成員變數的值。<br>
{% highlight python linenos %}
class Cat:
    name = None
    age = None
    color = None
    def getInfo(self, name):
        self.name = name

cat1 = Cat()
cat1.name = "小白"
cat1.age = 1
cat1.color = "while"
cat1.getInfo("小黃")
print(f"name = {cat1.name}")
{% endhighlight %}
```
name = 小黃
```