---
title: function
date: 2026-03-03
keywords: Python, function
---
## 語法
宣告函式由def開始，後面是函式名，然後圓括號()，圓括號裡面可以有參數，也可以沒有參數。<br>
圓括號後面有冒號:，函式的程式碼從冒號之後開始。<br>
使用縮排來撰寫函式中的程式碼。<br>

### 函式參數
有參數
```
def 函式名(參數1, 參數2...):分號
    程式碼
```

無參數
```
def 函式名():
    程式碼
```

### 函式傳回值
函式若沒有return，會有隱藏的return None<br>
[None](https://docs.python.org/zh-tw/3.14/library/constants.html#None)也是一個類型，代表「空」，但有自己的記憶體位址。<br>

{% highlight python linenos %}
def test():
    print("test")
result = test()
print("type of result = ",type(result))
print("result = ", result)
print("address = ", id(result))
{% endhighlight %}
```
test
type of result =  <class 'NoneType'>
result =  None
address =  4510668496
```

#### 有return
```
def 函式名(參數1, 參數2...):
    return 程式碼
```

#### 無return
```
def 函式名(參數1, 參數2...):
    程式碼
```

{% highlight python linenos %}
def max(a, b):
    if a > b:
        return a
    else:
        return b

n = max(10, 5)
print(n)
{% endhighlight %}
```
10
```

## 函式參數傳遞 記憶體位址
呼叫函式後，記憶體會開立一個「新的」Stack空間，跟主程式main的stack空間分開來。<br>

如果參數是基本型別(int, float)，而不是list、tuple……等等類型，<br>
函式是把傳進來的「記憶體位址」，複製給函式參數。

以下的程式碼執行結果為55，也就是函式中即便修改a參數為10，也不影嚮主程式的n，參數a跟變數n都在各別不同的Stack。
{% highlight python linenos %}
def test(a):
    a = 10

n = 55
test(n)
{% endhighlight %}
```
55
```

1.先執行主程式`n=55`，在Stack堆疊區中，建立新的Stack，n的變數存的是0x0011，Data資料區，0x0011記憶體位址存的是55。<br>
![img]({{site.imgurl}}/python/stack1.png)<br>

2.執行到test(a)函式後，在Stack堆疊區中，建立新的Stack，Stack建立方式由下至上。<br>
a的變數存的是0x0011。
![img]({{site.imgurl}}/python/stack2.png)<br>

3.`a=10`，會把變數a存0x0022的記憶體位址，0x0022記憶體位址存的是10。
![img]({{site.imgurl}}/python/stack3.png)<br>

4.test()函式執行完畢後，test函式的Stack會被清除，stack記憶體位址會被記憶體回收，供之後其它變數使用。<br>
![img]({{site.imgurl}}/python/stack4.png)<br>

函式內的變數，是屬於區域變數，離開函式無法再讀取，因為函式的記憶體位址在離開函式時已被清除。<br>

## 相同的函式名
Python可以有相同的函式名，執行函式時，執行離上方自己近的函式，不是下方。<br>
{% highlight python linenos %}
def test():
    print("1")

def test():
    print("2")

test()
{% endhighlight %}
```
2
```

{% highlight python linenos %}
def test():
    print("1")

test()
def test():
    print("2")

test()
{% endhighlight %}
```
1
2
```

## 注意事項
函式命名不能用數字跟`-`開頭。<br>
以下都會產生錯誤。<br>
{% highlight python linenos %}
def 1test():
    print("test")
def -test():
    print("test")
{% endhighlight %}


[1]: {% link _pages/c/function/callByValue.md %}




