---
title: 容器比較
date: 2026-03-16
keywords: Python, containers
---
容器(Containers)有list,tuple,str,set,dict。

|說明|list|tuple|str|set|dict|
|:---:|:---:|:---:|:---:|:---:|:---:|
|元素類型|任意類型|任意類型|字串|任意|key字串數字,value任意類型|
|元素能重覆|可|可|可|不可|key不可,value可|
|是否有順序|是|是|是|不是|3.6以後是|
|是否有索引|是|是|是|無|無|
|可修改|是|否|否|是|是|
|符號|`[]`|()|`""`|{}|{key:value}|


## 容器互相轉型
### list 轉 string
使用str(obj)函式，可以把list轉成字串，雖然執行結果`res: ['Hello', 'World', 'May']`有方括號，但這一串都是字串，後面的type有顯示res的類型為字串。<br>
{% highlight python linenos %}
list1 = ["Hello", "World", "May"]
res = str(list1)
print(f"res: {res} , type = {type(res)}")
{% endhighlight %}
```
res: ['Hello', 'World', 'May'] , type = <class 'str'>
```

### tuple 轉 string
{% highlight python linenos %}
tuple1 = ("Hello", "World", "May")
res = str(tuple1)
print(f"res: {res} , type = {type(res)}")
{% endhighlight %}
```
res: ('Hello', 'World', 'May') , type = <class 'str'>
```

### dict 轉 string
{% highlight python linenos %}
dict1 = {"key1": "value1", "key2": "value2"}
res = str(dict1)
print(f"res: {res} , type = {type(res)}")
{% endhighlight %}
```
res: {'key1': 'value1', 'key2': 'value2'} , type = <class 'str'>
```
### set 轉 string
{% highlight python linenos %}
set1 = {"H", "E", "H", "E"}
res = str(set1)
print(f"res: {res} , type = {type(res)}")
{% endhighlight %}
```
res: {'E', 'H'} , type = <class 'str'>
```

### 其它容器轉list
{% highlight python linenos %}
str1 = "Hello"
tuple1 = ("Mary", "Bill")
set1 = {"Mary", "Bill"}
dict1 = {"name": "Mary", "age": 25}
print(f"str1 convert list = {list(str1)}")
print(f"tuple1 convert list = {list(tuple1)}")
print(f"set1 convert list = {list(set1)}")
print(f"dict1 convert list = {list(dict1)}")
{% endhighlight %}
```
str1 convert list = ['H', 'e', 'l', 'l', 'o']
tuple1 convert list = ['Mary', 'Bill']
set1 convert list = ['Mary', 'Bill']
dict1 convert list = ['name', 'age']
```

### 其它容器轉tuple
{% highlight python linenos %}
str1 = "Hello"
list1 = [1,2,3,4,5]
tuple1 = ("Mary", "Bill")
set1 = {"Mary", "Bill"}
dict1 = {"name": "Mary", "age": 25}
print(f"str1 convert tuple = {tuple(str1)}")
print(f"list1 convert tuple = {tuple(list1)}")
print(f"set1 convert tuple = {tuple(set1)}")
print(f"dict1 convert tuple = {tuple(dict1)}")
{% endhighlight %}
```
str1 convert tuple = ('H', 'e', 'l', 'l', 'o')
list1 convert tuple = (1, 2, 3, 4, 5)
set1 convert tuple = ('Mary', 'Bill')
dict1 convert tuple = ('name', 'age')
```

### 其它容器轉set
{% highlight python linenos %}
str1 = "Hello"
list1 = [1,2,3,4,5]
tuple1 = ("Mary", "Bill")
set1 = {"Mary", "Bill"}
dict1 = {"name": "Mary", "age": 25}
print(f"str1 convert set = {set(str1)}")
print(f"list1 convert set = {set(list1)}")
print(f"tuple1 convert set = {set(tuple1)}")
print(f"dict1 convert set = {set(dict1)}")
{% endhighlight %}
```
str1 convert set = {'e', 'l', 'o', 'H'}
list1 convert set = {1, 2, 3, 4, 5}
tuple1 convert set = {'Mary', 'Bill'}
dict1 convert set = {'name', 'age'}
```

### 說明文件
[list](https://docs.python.org/zh-cn/3.12/library/stdtypes.html#list)

[tuple](https://docs.python.org/zh-cn/3.12/library/stdtypes.html#tuple)

[string](https://docs.python.org/zh-cn/3.12/library/stdtypes.html#string-methods)

[set](https://docs.python.org/zh-cn/3.12/library/stdtypes.html#set)

[dict](https://docs.python.org/zh-cn/3.12/library/stdtypes.html#dict)
