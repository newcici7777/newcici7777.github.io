---
title: set
date: 2026-03-13
keywords: Python, set
---
set是用花括號{}包起來。<br>
裡面的元素用逗號分開，不重覆，不按照順序。<br>
語法:<br>
```
{元素, 元素, 元素}
```
## 元素類型
Python 的 set（集合）只能儲存可雜湊（Hashable）且不可變（Immutable）的資料型別。元素只能數字、字串、元組（Tuple）等作為元素存入，但不能包含列表（List）、字典（Dictionary）或另一個集合（Set）等「可變類型」。每個元素必須是唯一的，使用雜湊表（Hash Table）實現高效率的尋找與去重。<br>

以下範例元素為字串、數字、元組Tuple。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100, (1, 2, 3)}
print(set1)
{% endhighlight %}
```
{'blue', 'red', 100, (1, 2, 3)}
```

使用list與dict會編譯錯誤。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100, [1, 2, 3]}
print(set1)
{% endhighlight %}
```
    set1 = {'blue', 'red', 100, [1, 2, 3]}
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: unhashable type: 'list'
```
### 可變與不可變
可變為可以修改、刪除。<br>
不可變為不可以修改，也不可以刪除，只能讀取。<br>

字串不可以修改，無法透過索引修改字串的單字。<br>
以下程式碼會編譯錯誤。<br>
{% highlight python linenos %}
str1 = "Hello"
st1[0] = "A"
{% endhighlight %}

## 不重覆
下方程式碼，有二個red，但執行之後，只有存一個red。<br>
如果要去掉重覆元素，可以使用set。<br>
{% highlight python linenos %}
set1 = {'blue','red','red'}
print(f"set1: {set1}, type(set1): {type(set1)}")
{% endhighlight %}
```
set1: {'red', 'blue'}, type(set1): <class 'set'>
```

## 元素無索引位置 無順序
以下的執行結果，是red先開始，再100，最後才是blue，跟宣告set的元素順序不同。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
for elem in set1:
    print(elem)
{% endhighlight %}
```
red
100
blue
```

因為set的元素取出是沒有順序，沒辦法使用索引來取得元素。<br>
以下執行時會錯誤。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
print(set1[0])
{% endhighlight %}

也無法使用while迴圈，因為沒辦法使用索引來取得元素。<br>


## 空set()
要宣告空的set，只能使用set()，不能呼叫花括號{}，因為花括號是建立空的dict。
{% highlight python linenos %}
set1 = set()
print(f"set1: {set1}, type(set1): {type(set1)}")
{% endhighlight %}
```
set1: set(), type(set1): <class 'set'>
```

## set操作函式
### len()
元素數量。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
print(len(set1))
{% endhighlight %}
```
3
```

### obj in set
判斷set是否有obj，回傳值為True或False。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
print('blue' in set1)
print(100 in set1)
{% endhighlight %}
```
True
True
```

### set.add(obj) 新增
把obj新增至set中。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
set1.add(55.5)
print(set1)
{% endhighlight %}
```
{100, 55.5, 'blue', 'red'}
```

### set.remove(obj) 刪除
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
set1.remove(100)
print(set1)
{% endhighlight %}
```
{'blue', 'red'}
```

若無obj元素，會產生KerError。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
set1.remove(1)
print(set1)
{% endhighlight %}
```
    set1.remove(1)
KeyError: 1
```

### pop 取出元素並刪除元素
pop會刪除元素，所以再遍歷一次set，就會少一個元素。<br>
{% highlight python linenos %}
set1 = {'blue', 'red', 100}
elem = set1.pop() # 取出元素並刪除元素
print(f"elem = {elem}, type = {type(elem)}")
print(f"set1: {set1}")
{% endhighlight %}
```
elem = blue, type = <class 'str'>
set1: {100, 'red'}
```

### union 合併
union可以合併二個set，並且產生新的set。<br>
{% highlight python linenos %}
set1 = {"a", "b", "c"}
set2 = {1, 2, 3}
set3 = set1.union(set2)
print(set3)
{% endhighlight %}
```
{1, 'c', 'b', 2, 3, 'a'}
```

以下的程式碼，跟上面的程式碼是相同的。<br>
{% highlight python linenos %}
set1 = {"a", "b", "c"}
set2 = {1, 2, 3}
set3 = set1 | set2
print(set3)
{% endhighlight %}

### intersection 交集
取出二個集合set中，共同擁有的元素。<br>
{% highlight python linenos %}
set1 = {"a", "b", "c"}
set2 = {"d", "a", "b"}
set3 = set1.intersection(set2)
print(set3)
{% endhighlight %}
```
{'b', 'a'}
```

以下的程式碼，跟上面的程式碼是相同的。<br>
{% highlight python linenos %}
set1 = {"a", "b", "c"}
set2 = {"d", "a", "b"}
set3 = set1 & set2
print(set3)
{% endhighlight %}


### difference 差集
二個集合把相同的元素刪掉，傳回前一個集合剩下的元素。<br>

前一個集合 - 後面的集合 = 去掉相同元素，傳回前一個集合剩下的元素。<br>

以下程式碼，a與b是二個集合共同擁有，set1刪掉a、b，傳回剩下的c。
{% highlight python linenos %}
set1 = {"a", "b", "c"}
set2 = {"d", "a", "b"}
set3 = set1 - set2
print(set3)
{% endhighlight %}
```
{'c'}
```

以下程式碼，跟上面的程式碼是相同的。<br>
{% highlight python linenos %}
set1 = {"a", "b", "c"}
set2 = {"d", "a", "b"}
set3 = set1.difference(set2)
print(set3)
{% endhighlight %}

若把set2作為「前一個集合」去減 set1，會傳回set2扣掉相同元素後，set2剩下的元素。<br>
{% highlight python linenos %}
set1 = {"a", "b", "c"}
set2 = {"d", "a", "b"}
set3 = set2 - set1
print(set3)
{% endhighlight %}
```
{'d'}
```

## Comprehension運算式 建立set
由於set是無順序，產生出來的set，不會按照下圖的順序產生。<br>

![img]({{site.imgurl}}/python/set_compreh.png)

{% highlight python linenos %}
set1 = {elem * elem for elem in [1, 2, 3, 4]}
print(set1)
{% endhighlight %}
```
{16, 1, 4, 9}
```




