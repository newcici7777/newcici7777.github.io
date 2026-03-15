---
title: tuple
date: 2026-03-15
keywords: Python, tuple
---
- tuple是不可新增修改，只能唯讀。<br>
- tuple是有順序，所以可以使用索引位置讀取元素。<br>
- tuple的元素可以重覆。<br>

## 宣告
使用圓括號與逗號建立tuple。<br>
{% highlight python linenos %}
tuple1 = (1, 2, "Hello", ['Hi', 55.5])
print(f"tuple1: {tuple1}, type = {type(tuple1)}")
{% endhighlight %}
```
tuple1: (1, 2, 'Hello', ['Hi', 55.5]), type = <class 'tuple'>
```

元素可以為任意類型，可以為list`[]`、tuple、set、dict、int、string、float。<br>

### 空tuple
使用圓括號 () 或 tuple()，建立空的tuple。<br>
{% highlight python linenos %}
tuple1 = ()
tuple2 = tuple()
print(f"tuple1: {tuple1}, type = {type(tuple1)}")
print(f"tuple2: {tuple2}, type = {type(tuple2)}")
{% endhighlight %}
```
tuple1: (), type = <class 'tuple'>
tuple2: (), type = <class 'tuple'>
```

### tuple只有一個元素 後面要逗號
tuple只有一個元素，若後面不加上逗號，類型為int。<br>
{% highlight python linenos %}
tuple1 = (1)
print(f"tuple1: {tuple1} , type = {type(tuple1)}")
{% endhighlight %}
```
tuple1: 1 , type = <class 'int'>
```

加上逗號，類型為tuple。<br>
{% highlight python linenos %}
tuple1 = (1,)
print(f"tuple1: {tuple1} , type = {type(tuple1)}")
{% endhighlight %}
```
tuple1: (1,) , type = <class 'tuple'>
```

## 讀取元素
透過`[索引]`可以取得元素。<br>
{% highlight python linenos %}
tuple1 = (1, 2, 3)
print(f"tuple1[0]: {tuple1[0]}")
print(f"tuple1[1]: {tuple1[1]}")
print(f"tuple1[2]: {tuple1[2]}")
{% endhighlight %}
```
tuple1[0]: 1
tuple1[1]: 2
tuple1[2]: 3
```

### 讀取元素為list
{% highlight python linenos %}
tuple1 = (1, 2, ["H", 'E', "LL"])
print(f"tuple1[2][0] = {tuple1[2][0]}")
print(f"tuple1[2][1] = {tuple1[2][1]}")
print(f"tuple1[2][2] = {tuple1[2][2]}")
{% endhighlight %}
```
tuple1[2][0] = H
tuple1[2][1] = E
tuple1[2][2] = LL
```

### 讀取元素為tuple
{% highlight python linenos %}
tuple1 = (1, 2, ("H", 'E', "LL"))
print(f"tuple1[2][0] = {tuple1[2][0]}")
print(f"tuple1[2][1] = {tuple1[2][1]}")
print(f"tuple1[2][2] = {tuple1[2][2]}")
{% endhighlight %}
```
tuple1[2][0] = H
tuple1[2][1] = E
tuple1[2][2] = LL
```

### 讀取元素為dict
注意！dict是要用key來讀取，而不是索引，dict不支持索引功能。<br>
{% highlight python linenos %}
tuple1 = (1, 2, {"s01":"Mary", "s02": "Bill"})
print(f"tuple1[2][key] = {tuple1[2]["s01"]}")
print(f"tuple1[2][key] = {tuple1[2]["s02"]}")
{% endhighlight %}
```
tuple1[2][key] = Mary
tuple1[2][key] = Bill
```

### 讀取元素為set
因為set不是順序，所以無法使用索引讀取。<br>

## 迴圈遍歷
### for
{% highlight python linenos %}
tuple1 = (1, 2, 3)
for elem in tuple1:
    print(elem)
{% endhighlight %}
```
1
2
3
```

### while
因為tuple是有順序，可以使用while 順序透過索引位置讀取每個元素。<br>
{% highlight python linenos %}
tuple1 = (1, 2, 3)
i = 0
while i < len(tuple1):
    print(f"tuple[{i}] = {tuple1[i]}")
    i += 1
{% endhighlight %}
```
tuple[0] = 1
tuple[1] = 2
tuple[2] = 3
```

## 重覆元素
可以有重覆的元素。<br>
{% highlight python linenos %}
tuple1 = (1, 1, 1)
for item in tuple1:
    print(item)
{% endhighlight %}
```
1
1
1
```

## 無法修改 刪除
tuple是唯讀，無法修改刪除元素。<br>
無法修改。<br>
{% highlight python linenos %}
tuple1 = (1, 1, 1)
tuple1[0] = 100
{% endhighlight %}
```
    tuple1[0] = 100
    ~~~~~~^^^
TypeError: 'tuple' object does not support item assignment
```

無法刪除。<br>
{% highlight python linenos %}
tuple1 = (1, 1, 1)
del tuple1[0]
{% endhighlight %}
```
    del tuple1[0]
        ~~~~~~^^^
TypeError: 'tuple' object doesn't support item deletion
```

