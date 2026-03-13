---
title: dict
date: 2026-03-13
keywords: Python, dict
---
dict，中文是字典，英文是dictionary。<br>

## 語法
dict是由花括號{}包住元素。<br>
每一個元素都是key :冒號 value 所組成。<br>
```
dict1 = {key1: value1, key2: value2, key3: value3}
```

### 空字典
{% highlight python linenos %}
dict1 = {}
dict2 = dict()
print(f"dict1={dict1} type(dict1)={type(dict1)}")
print(f"dict2={dict2} type(dict2)={type(dict2)}")
{% endhighlight %}
```
dict1={} type(dict1)=<class 'dict'>
dict2={} type(dict2)=<class 'dict'>
```

## key
key只能是數字或字串。<br>

以下的 key 為數字，輸出的類型為dict。<br>
{% highlight python linenos %}
dict1 = {11: "Mary", 12: "Bill", 6: "John"}
print(f"dict1: {dict1}, type(dict1): {type(dict1)}")
{% endhighlight %}
```
dict1: {11: 'Mary', 12: 'Bill', 6: 'John'}, type(dict1): <class 'dict'>
```

以下的 key 為字串。
{% highlight python linenos %}
dict2 = {"001": "Mary", "002": "Bill", "003": "John"}
{% endhighlight %}

### 透過key取出內容
使用方括號[key]取出內容。
```
字典變數名[key]
dict1[11]
```

{% highlight python linenos %}
dict1 = {11: "Mary", 12: "Bill", 6: "John"}
print(f"11 = {dict1[11]}")
print(f"12 = {dict1[12]}")
print(f"6 = {dict1[6]}")
{% endhighlight %}
```
11 = Mary
12 = Bill
6 = John
```

- [string 字串][1]

若key為字串，可以使用雙引號或「單引號」包住key，單引號也是字串類型。<br>
以下程式碼的key都是字串類型。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
print(f" s01 = {dict1['s01']}")
print(f" s02 = {dict1["s02"]}")
print(f" s02 = {dict1["""s02"""]}")
print(f" s01 = {dict1['''s01''']}")
{% endhighlight %}
```
 s01 = Mary
 s02 = Bill
 s02 = Bill
 s01 = Mary
```

### 相同的key
若有相同的key，會取出位置在後方的key。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill", "s03": "John", "s01": "Bobo"}
print(dict1["s01"])
{% endhighlight %}
```
Bobo
```

## value可以為任何類型
key只能是數字或字串，但value可以為任何類型。<br>
{% highlight python linenos %}
dict1 = {
    "Mary": {"City": "Tokyo"},
    "Bill": 10,
    "Alex": [11, 20, 30]
}
print(dict1)
{% endhighlight %}
```
{'Mary': {'City': 'Tokyo'}, 'Bill': 10, 'Alex': [11, 20, 30]}
```
## dict不能用index索引位置
若想取出索引位置為 0 的值，以下會有KeyError錯誤。<br>
{% highlight python linenos %}
dict1 = {11: "Mary", 12: "Bill", 6: "John"}
print(dict1[0])
{% endhighlight %}
```
    print(dict1[0])
          ~~~~~^^^
KeyError: 0
```

## 迴圈
dict不支援while，只支援for。<br>

### in 字典變數
以下程式碼，`in dict1`，dict1為字典變數。<br>
{% highlight python linenos %}
dict1 = {11: "Mary", 12: "Bill", 6: "John"}
# 方式1
for key in dict1:
    print(f"key = {key}, value = {dict1[key]}")
{% endhighlight %}
```
key = 11, value = Mary
key = 12, value = Bill
key = 6, value = John
```

### keys() values()
透過keys()與values()可以取得key與value。<br>

keys()範例如下:
{% highlight python linenos %}
dict1 = {11: "Mary", 12: "Bill", 6: "John"}
# 方式2
for key in dict1.keys():
    print(f"key = {key}, value = {dict1[key]}")
{% endhighlight %}
```
key = 11, value = Mary
key = 12, value = Bill
key = 6, value = John
```

value()範例如下:
{% highlight python linenos %}
for value in dict1.values():
    print(f"value = {value}")
{% endhighlight %}
```
value = Mary
value = Bill
value = John
```

### items()
{% highlight python linenos %}
dict1 = {11: "Mary", 12: "Bill", 6: "John"}
# 方式3
for key, value in dict1.items():
    print(f"key = {key}, value = {value}")
{% endhighlight %}

## dict操作函式
### len(dict)
key元素數量。<br>

若key有重覆，會去掉重覆的數量，以下的結果，數量為1。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill", "s03": "John", "s01": "Bobo"}
print(len(dict1))
{% endhighlight %}
```
3
```

### 新增修改
語法
```
dict[key] = value
```

以下程式碼，若 key 沒有在dict中，就是新增。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
dict1["s05"] = "John"
print(dict1)
{% endhighlight %}
```
{'s01': 'Mary', 's02': 'Bill', 's05': 'John'}
```

若 key 已存在dict中，就是修改。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
dict1["s01"] = "Sisi"
print(dict1)
{% endhighlight %}
```
{'s01': 'Sisi', 's02': 'Bill'}
```

### 刪除 del
語法
```
del dict[key]
```

若key不存在dict中，會KeyError。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
del dict1["s01"]
print(dict1)
{% endhighlight %}
```
{'s02': 'Bill'}
```

### pop()
語法
```
dict.pop(key, default)
```

若 key 不存在，傳回default ， default 參數可不填。<br>
沒有default，dict.pop(key)，dict沒有key，則會KeyError。<br>
pop(key)，先把value取出，再執行 del dict[key] 刪除元素。<br>

{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
data = dict1.pop("s01")
print(f"data: {data}")
print(f"dict1 = {dict1}")
{% endhighlight %}
```
data: Mary
dict1 = {'s02': 'Bill'}
```

{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
data = dict1.pop("s07", "Not found")
print(f"data: {data}")
{% endhighlight %}
```
data: Not found
```

### keys()
取出所有的key，傳回值類型為dict_keys。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
keys = dict1.keys()
print(f"keys = {keys} type = {type(keys)}")
{% endhighlight %}
```
keys = dict_keys(['s01', 's02']) type = <class 'dict_keys'>
```

類型為dict_keys，可使用for 取出內容。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
keys = dict1.keys()
for key in keys:
    print(key)
{% endhighlight %}
```
s01
s02
```

### key in dict
key為要搜索的key，若有此key，傳回True，找不到傳回False。<br>
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
print(f" find s07 = {"s07" in dict1}")
print(f" find s02 = {"s02" in dict1}")
{% endhighlight %}
```
 find s07 = False
 find s02 = True
```

### dict.clear() 刪除所有元素
{% highlight python linenos %}
dict1 = {"s01": "Mary", "s02": "Bill"}
dict1.clear()
print(dict1)
{% endhighlight %}
```
{}
```

## zip()
使用zip可以把2個list合併成dict。<br>

語法
```
{k:v for k,v in zip[list1, list2]}
```
以上的2個k 與v 要相同，k與v皆為變數名，可自由替換，非固定。<br>

![img]({{site.imgurl}}/python/dict_comprehen.png)

步驟如下:
1. 取出`list1[0]` 與 `list2[0]`的索引為0的元素。
2. `list1[0]`指派給k `list[1]`指派給v
3. k再指派給k v再指派給v
4. 透過k、v，產生dict。

{% highlight python linenos %}
list1 = ["s01", "s02"]
list2 = ["Mary", "Bill"]
dict2 = {ele1:ele2 for ele1, ele2 in zip(list1, list2)}
dict1 = {k:v for k,v in zip(list1,list2)}
print(dict1)
{% endhighlight %}
```
{'s01': 'Mary', 's02': 'Bill'}
```

[1]: {% link _pages/python/string.md %}
