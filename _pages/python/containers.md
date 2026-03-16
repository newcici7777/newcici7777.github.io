---
title: 容器
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
