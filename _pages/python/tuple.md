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

### 讀取元素為負索引
-1為tuple的最後一個，-2為tuple的倒數第二個，依此類推。<br>
{% highlight python linenos %}
tuple1 = (1, 2, 3)
print(f"tuple1[-1]: {tuple1[-1]}")
print(f"tuple1[-2]: {tuple1[-2]}")
print(f"tuple1[-3]: {tuple1[-3]}")
{% endhighlight %}
```
tuple1[-1]: 3
tuple1[-2]: 2
tuple1[-3]: 1
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

## 元素為list
若元素為list、dict、set，就可以新增/修改/刪除裡面的內容。<br>

tuple的元素為list，可以修改list\[0\]的內容。<br>
{% highlight python linenos %}
tuple1 = (1,["H","E"])
tuple1[1][0] = "A"
print(tuple1)
{% endhighlight %}

但如果要把list\[0\]指向其它list的記憶體位址，執行時會產生錯誤，因為tuple的元素是無法修改。<br>

{% highlight python linenos %}
tuple1 = (1,["H","E"])
tuple1[1] = ["Hello", "World"]
print(tuple1)
{% endhighlight %}
```
    tuple1[1] = ["Hello", "World"]
    ~~~~~~^^^
TypeError: 'tuple' object does not support item assignment
```

刪除list的第0個索引。<br>
{% highlight python linenos %}
tuple1 = (1,["H","E"])
del tuple1[1][0]
print(tuple1)
{% endhighlight %}
```
(1, ['E'])
```

新增list的元素。<br>
{% highlight python linenos %}
tuple1 = (1,["H","E"])
tuple1[1].append("J")
print(tuple1)
{% endhighlight %}
```
(1, ['H', 'E', 'J'])
```

## tuple常用操作函式

|函式名|說明|
|:-----:|:--------------|
|len(tuple)|元素數量|
|max(tuple)|元素最大值|
|min(tuple)|元素最小值|
|tuple.count(obj)|計算obj出現在tuple的次數|
|tuple.index(obj)|尋找obj在tuple中的索引位置|
|obj in tuple|判斷obj是否在tuple中|

{% highlight python linenos %}
tuple1 = (1, 2, 3, 1, 2)
# 元素數量
print(f"len = {len(tuple1)}")
# 元素最大值
print(f"max = {max(tuple1)}")
# 元素最小值
print(f"min = {min(tuple1)}")
# 計算obj出現在tuple的次數
print(f"count = {tuple1.count(1)}")
# 尋找obj在tuple中的索引位置
print(f"index = {tuple1.index(3)}")
{% endhighlight %}
```
len = 5
max = 3
min = 1
count = 2
index = 2
```

{% highlight python linenos %}
tuple1 = (1, 2, 3, 1, 2)
print(3 in tuple1)
{% endhighlight %}
```
True
```
