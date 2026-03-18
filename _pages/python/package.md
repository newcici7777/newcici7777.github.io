---
title: package 套件
date: 2026-03-17
keywords: Python, package
---
點選專案名稱，滑鼠右鍵，New -> Python Package <br>
![img]({{site.imgurl}}/python/package1.png)<br>

自己取package名字。<br>
![img]({{site.imgurl}}/python/package2.png)<br>

會產生一個 `__init__.py` 檔案。<br>
![img]({{site.imgurl}}/python/package3.png)<br>

在我自建的package下，建立一個module1.py檔案，內容如下:<br>

module1.py:<br>
{% highlight python linenos %}
def add(x, y):
    return x + y

def minus(x, y):
    return x - y

def div(x, y):
    return x / y

def mul(x, y):
    return x * y
{% endhighlight %}

新增一個module2.py檔案，內容如下:<br>
{% highlight python linenos %}
def hi():
    print("hi")
    
def hello():
    print("hello")
{% endhighlight %}

## import 套件.模組
在my_package之外，建立一個test1.py，用到呼叫套件中模組的函式。<br>

目錄結構如下:
```
專案
|-- my_package
|     |- module1.py
|     |- module2.py
|-- test1.py
```

語法:
```
import 套件.模組
# 呼叫函式
套件.模組.函式()
```

以下程式碼是test1.py 呼叫套件中的模組函式。<br>
麻煩的是，呼叫函式的時候，套件也要有。<br>

test1.py
{% highlight python linenos %}
# import 套件 與 模組
import my_package.module1
# 呼叫函式
print(my_package.module1.add(1, 2))
{% endhighlight %}
```
3
```

## from 套件 import 模組
語法:<br>
```
from 套件 import 模組
# 呼叫函式
模組.函式()
```

呼叫模組函式時，不用有套件名。<br>

test1.py
{% highlight python linenos %}
from my_package import module1
print(module1.add(1, 2))
{% endhighlight %}

## from 套件.模組 import 函式
語法:<br>
```
from 套件.模組 import 函式
# 呼叫函式
函式()
```

以下用 * 星號取代函式，代表所有函式都會被import進來。<br>
{% highlight python linenos %}
from my_package.module1 import *
print(add(1, 2))
{% endhighlight %}

## from 套件 import *
在 `__init__.py` 中，增加 `__all__` 可以限制那些模組可以使用。<br>
{% highlight python linenos %}
__all__ = ["module1", "module2"]
{% endhighlight %}

使用星號 * 匯入所有模組。<br>
{% highlight python linenos %}
from my_package import *

module2.hi()
print(module1.add(5, 10))
{% endhighlight %}
```
hi
15
```

若模組沒加到 `__all__` 中，使用星號 * 時，無法使用。<br>
`__init__.py` `__all__` 去除 module1 <br>
{% highlight python linenos %}
__all__ = ["module2"]
{% endhighlight %}

再次執行以下程式碼，module1就會有錯誤。<br>
{% highlight python linenos %}
from my_package import *

module2.hi()
print(module1.add(5, 10))
{% endhighlight %}

以上僅對`from 套件 import *` 有效。<br>
`from 套件 import 模組`，就不會受到 `__init__.py` `__all__` 影嚮。<br>

以下程式碼，使用`from my_package import module1`，即便 `__all__` 只有 module2 可以對外呼叫，但仍是可以使用 module1。

test1.py
{% highlight python linenos %}
from my_package import module1
from my_package import module2

module2.hi()
print(module1.add(5, 10))
{% endhighlight %}
```
hi
15
```

## 套件中有套件
套件中可以有子套件。<br>
下圖中，my_package2套件下面有child_package1套件。<br>
![img]({{site.imgurl}}/python/package4.png)<br>

呼叫方式有以下二種。<br>
方法1:<br>
```
form 套件.套件.模組 import 函式
函式()
```

方法2:<br>
```
from 套件.套件 import 模組
模組.函式()
```



