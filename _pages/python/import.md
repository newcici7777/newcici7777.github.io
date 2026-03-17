---
title: import
date: 2026-03-10
keywords: Python, function import
---
模組就是寫一個py檔案，把許多函式放在裡面，這個py檔案就是模組。

使用前要import。<br>

## import 其它模組的函式
test2.py檔案 內容如下:
{% highlight python linenos %}
def add(x, y):
    return x + y
{% endhighlight %}

模組語法:
```
import 檔名
```
檔名不包含.py

建立一個新的test1.py檔案，程式碼內容如下:<br>
{% highlight python linenos %}
# import test2檔名(不包含.py)
import test2

# 使用test2.py檔案中 add() 函式
result = test2.add(10, 20)
print(result)
{% endhighlight %}
```
30
```

## import 標準函式庫
Python有提供標準函式庫。<br>

請在以下搜尋「標準函式」<br>
<https://docs.python.org/zh-tw/3.12/tutorial/index.html>

{% highlight python linenos %}
import math
import random

# 取出絕對值
print(math.fabs(-100))
# 從list中隨機傳回其中的元素
print(random.randint(1,100))
{% endhighlight %}
```
100.0
48
```

也可以把 import 寫在一起。<br>
{% highlight python linenos %}
import math,random
{% endhighlight %}

## from 模組 import 函式
上一個呼叫函式，需要`模組.函式()`。<br>

使用from 模組 import 函式，就直接呼叫函式()就可以。<br>
{% highlight python linenos %}
from random import choice
print(choice(['Hello', 'Marry', 'Happy']))
{% endhighlight %}

## from 模組 import *
使用 * 號，就可以匯入所有函式。<br>
{% highlight python linenos %}
from random import *
print(choice(['Hello', 'Marry', 'Happy']))
# 產生 1 - 100(包含100) 之間的數字
print(randint(1, 100))
{% endhighlight %}
```
Marry
91
```

## import 模組 as 名稱
可以為模組取客制化的名稱。<br>
以下程式碼，r就是自己取的名稱。<br>
{% highlight python linenos %}
import random as r
print(r.choice(['Hello', 'Marry', 'Happy']))
# 產生 1 - 100(包含100) 之間的數字
print(r.randint(1, 100))
{% endhighlight %}
```
Marry
91
```

## from 模組 import 函式 as 名稱
可以為函式取客制化的名稱。<br>
{% highlight python linenos %}
from random import choice as mychoice
print(mychoice(['Hello', 'Marry', 'Happy']))
{% endhighlight %}

## __name__
test2.py 輸出__name__
{% highlight python linenos %}
def add(x, y):
    return x + y
print("test2.py :" + __name__)
{% endhighlight %}

test1.py 輸出__name__
{% highlight python linenos %}
import test2

print(test2.add(5,6))
print("test1.py :" + __name__)
{% endhighlight %}

執行test1.py
```
test2.py :test2
11
test1.py :__main__
```

由上面的結果可以發現，test1.py 呼叫 test2模組中的 add() 函式。<br>
test1.py 是呼叫方， __name__ 輸出為 __main__ 。<br>
test2.py 是被呼叫方， __name__ 輸出為 test2 。<br>

由此可知，呼叫函式的那一方，輸出的都是 __main__ 代表它是「主要」函式。<br>

### 測試模組
如果開發test2模組時，在test2模組呼叫add()函式，但忘記註解。<br>

test2.py
{% highlight python linenos %}
def add(x, y):
    return x + y

# 測試用
print(add(100, 50))
{% endhighlight %}

test1.py 使用test2模組，並呼叫add()函式。<br>
{% highlight python linenos %}
import test2

print(test2.add(5,6))
{% endhighlight %}

執行結果就會把測試用`print(add(100, 50))`，也印出來。<br>
```
150
11
```

如何讓主函式呼叫模組時，測試用的程式碼不被呼叫呢？但模組自己測試的時候又可以呼叫。<br>

使用 __name__ 可以分辦執行函式的人是誰，若執行函式的人是自己， __name__ 就會是main，如果是作為模組函式執行， __name__ 就會是模組名test2。<br>

test2.py
{% highlight python linenos %}
def add(x, y):
    return x + y

# 測試用
if __name__ == '__main__':
    print(add(100, 50))
{% endhighlight %}

## __all__
__all__ 只能用在` from 模組 import 函式`

設定要開放那些函式給其它人呼叫。<br>

test2.py 有add()、minus()、div()、mul()，但利用 __all__，只開放add()函式供其它人使用。<br>
{% highlight python linenos %}
__all__ = ['add']
def add(x, y):
    return x + y

def minus(x, y):
    return x - y

def div(x, y):
    return x / y

def mul(x, y):
    return x * y
{% endhighlight %}

test1.py 使用`from 模組 import 函式`的方式匯入test2所有方法。<br>
以下程式碼使用minus()會編譯時期的錯誤。<br>
{% highlight python linenos %}
from test2 import *

print(add(5,6))
print(minus(5,6))
{% endhighlight %}

但若使用`import 模組`，則可以使用test2中所有函式。<br>
{% highlight python linenos %}
import test2

print(test2.add(5,6))
print(test2.minus(5,6))
{% endhighlight %}
```
11
-1
```

## ctrl + b 查看模組所有函式
滑鼠移到下方「math」，windows按 ctrl + b , mac 按 cmd + b，就會進入模組文件 
{% highlight python linenos %}
import math
{% endhighlight %}

點擊下圖Structure ，就會展示 module 所有函式。<br>
![img]({{site.imgurl}}/python/module1.png)<br>
![img]({{site.imgurl}}/python/module2.png)<br>