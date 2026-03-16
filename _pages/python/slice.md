---
title: slice
date: 2026-03-16
keywords: Python, slice
---
slice適用於有順序且可以用索引位置取得元素的資料結構，如: list, tuple, string。<br>

## 語法
```
有順序的資料結構[開始索引:結束索引:step]
```
使用分號:區隔。<br>

會取出的索引介於 開始索引 <= 索引 < 結束索引。<br>

注意！不包含結束索引。<br>

開始索引不寫，預設為0。<br>

結束索引不寫，預設為字串的長度或list的長度。<br>

step預設為1，可以不填，預設為1就是一個一個取出來，若step為2，會間隔一格，再取。<br>

## 字串
字串使用slice，傳回的類型為新的字串，不會影嚮原本的字串。<br>

語法:
```
字串[開始索引:結束索引:step]
```

### 間隔為1
以下的程式碼會取出索引1至2的字母，不包含結束索引3的字串。<br>
step不寫，預設為1。<br>

|索引|0|1|2|3|4|
|:---:|:---:|:---:|:---:|:---:|:---:|
|字串|H|e|l|l|o|
|取出||x|x|||

{% highlight python linenos %}
str1 = "Hello"
result = str1[1:3]
print(f"result = {result}, type = {type(result)}")
{% endhighlight %}
```
result = el, type = <class 'str'>
```

### 間隔為2
以下取出索引0到4，不包含結束索引5，間隔為二格。<br>

|索引|0|1|2|3|4|
|:---:|:---:|:---:|:---:|:---:|:---:|
|字串|H|e|l|l|o|
|取出|x||x||x|

{% highlight python linenos %}
str1 = "Hello"
result = str1[0:5:2]
print(f"result = {result}, type = {type(result)}")
{% endhighlight %}
```
result = Hlo, type = <class 'str'>
```

### 結束索引不寫
結束索引不寫，預設為字串長度len(str)。<br>

|索引|0|1|2|3|4|
|:---:|:---:|:---:|:---:|:---:|:---:|
|字串|H|e|l|l|o|
|取出|||x|x|x|

{% highlight python linenos %}
str1 = "Hello"
result = str1[2:]
print(f"result = {result}, type = {type(result)}")
{% endhighlight %}
```
result = llo, type = <class 'str'>
```

### 開始索引不寫
開始索引不寫，預設為0，結束索引不寫，預設為字串長度 len(str) 。<br>

|索引|0|1|2|3|4|
|:---:|:---:|:---:|:---:|:---:|:---:|
|字串|H|e|l|l|o|
|取出|x||x||x|

{% highlight python linenos %}
str1 = "Hello"
result = str1[::2]
print(f"result = {result}")
{% endhighlight %}
```
result = Hlo
```

### 反向截取
若開始索引為-1，代表從尾部開始取值。<br>
若開始索引為負，結束索引與step也要為負。<br>

|索引|-5|-4|-3|-2|-1|
|:---:|:---:|:---:|:---:|:---:|:---:|
|字串|H|e|l|l|o|
|取出|x|x|x|x|x|

以下從尾部索引-1開始取值，直到-5結束。<br>
{% highlight python linenos %}
str1 = "Hello"
result = str1[-1:-6:-1]
print(f"result = {result}")
{% endhighlight %}
```
result = olleH
```

若結束索引不填，此範例預設為-6。<br>
{% highlight python linenos %}
str1 = "Hello"
result = str1[-1::-1]
print(f"result = {result}")
{% endhighlight %}
```
result = olleH
```

間隔為-2。<br>

|索引|-5|-4|-3|-2|-1|
|:---:|:---:|:---:|:---:|:---:|:---:|
|字串|H|e|l|l|o|
|取出|x||x||x|

{% highlight python linenos %}
str1 = "Hello"
result = str1[-1::-2]
print(f"result = {result}")
{% endhighlight %}
```
result = olH
```

## list
除了string外，也可以使用在list中。<br>

取得開始索引為1至3，不包含索引4，間隔為1，傳回值類型為list。<br>
{% highlight python linenos %}
list1 = ["Tom", "Joy", "Jack", "Mary", "Alex", "Alice"]
newlist = list1[1:4]
print(f"newlist: {newlist}, type = {type(newlist)} ")
{% endhighlight %}
```
newlist: ['Joy', 'Jack', 'Mary'], type = <class 'list'> 
```

反向取得元素。<br>
開始索引為-1，結束索引為-7，間隔為-2。
{% highlight python linenos %}
list1 = ["Tom", "Joy", "Jack", "Mary", "Alex", "Alice"]
newlist = list1[-1::-2]
print(f"newlist: {newlist} ")
{% endhighlight %}
```
newlist: ['Alice', 'Mary', 'Joy']
```

## list.reverse() 翻轉與 slice 比較
使用list.reverse()會把原list的元素全部翻轉。<br>
{% highlight python linenos %}
list1 = ["Tom", "Joy", "Jack", "Mary", "Alex", "Alice"]
list1.reverse()
print(f"list1 = {list1} ")
{% endhighlight %}
```
list1 = ['Alice', 'Alex', 'Mary', 'Jack', 'Joy', 'Tom'] 
```

使用 slice 不會影嚮原本的list，會傳回「新」的list。<br>
{% highlight python linenos %}
list1 = ["Tom", "Joy", "Jack", "Mary", "Alex", "Alice"]
print(f"slice = {list1[-1::-1]} ")
print(f"list1 = {list1}")
{% endhighlight %}
```
slice = ['Alice', 'Alex', 'Mary', 'Jack', 'Joy', 'Tom'] 
list1 = ['Tom', 'Joy', 'Jack', 'Mary', 'Alex', 'Alice']
```

## tuple
把前一個範例改成tuple，也適用於tuple。<br>
{% highlight python linenos %}
tuple1 = ("Tom", "Joy", "Jack", "Mary", "Alex", "Alice")
print(f"slice = {tuple1[-1::-1]} ")
print(f"list1 = {tuple1}")
{% endhighlight %}
```
slice = ('Alice', 'Alex', 'Mary', 'Jack', 'Joy', 'Tom') 
list1 = ('Tom', 'Joy', 'Jack', 'Mary', 'Alex', 'Alice')
```