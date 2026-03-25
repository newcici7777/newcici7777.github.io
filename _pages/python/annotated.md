---
title: 標註變數的類型Annotated
date: 2026-03-25
keywords: Python, Annotated
---
Python的變數是沒有類型的，可以使用標註Annotated，知道變數的類型。<br>

標註應用範圍:
- 變數類型
- 參數類型
- 傳回值類型
- 類別

## 語法
使用冒號來區分類型。
```
變數: 類型
```

## 標註參數
函式參數希望是str，但傳進來是整數，就會有警告。<br>
{% highlight python linenos %}
def func1(param1: str):
    print(param1)

func1(10)
{% endhighlight %}

警告的圖片如下:<br>
![img]({{site.imgurl}}/python/annotated1.png)<br>

在參數按下鍵盤ctrl + p (mac: cmd + p) 就會提示參數類型。<br>
![img]({{site.imgurl}}/python/annotated2.png)<br>

## 標註變數
{% highlight python linenos %}
i: int = 0
b1: bool = False
f1: float = 1.0
s1: str = ""
{% endhighlight %}

## 標註類別
{% highlight python linenos %}
class Dog:
    pass

dog1:Dog = Dog()
{% endhighlight %}

## 標註容器
{% highlight python linenos %}
list1: list = []
tuple1: tuple = ()
dict1: dict = {}
set1: set = set()
{% endhighlight %}

可以為每個元素設類型。<br>
{% highlight python linenos %}
list1: list[str] = ["abc", "def"]
tuple1: tuple[int, str, float, bool] = (100, "Hello", 3.5, True)
dict1: dict[str, float] = {"a": 1, "b": 2, "c": 3}
set1: set[str] = {"a", "b", "c"}
{% endhighlight %}

## 註解標註
語法
```
# type:類型
```
{% highlight python linenos %}
a = "hello"  # type:float
s = 10 # type:str
{% endhighlight %}


## 傳回值標註
語法:<br>
```
def 函式名(參數名: 參數類型, 參數名: 參數類型) -> 傳回值類型:
	程式碼
```
{% highlight python linenos %}
def func2(a: int, b: int, c: int) -> str:
    return f"{a} {b} {c}"
{% endhighlight %}


## Union
使用前要匯入 Union
```
from typing import Union
```

語法
```
Union[類型, 類型, ..]
```

Union[int, str]意思為類型可以是int，也可以是str。<br>
{% highlight python linenos %}
from typing import Union

data: Union[int, str] = "abc"
{% endhighlight %}