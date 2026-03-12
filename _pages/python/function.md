---
title: function
date: 2026-03-03
keywords: Python, function
---
## 語法
宣告函式由def開始，後面是函式名，然後圓括號()，圓括號裡面可以有參數，也可以沒有參數。<br>
圓括號後面有冒號:，函式的程式碼從冒號之後開始。<br>
使用縮排來撰寫函式中的程式碼。<br>

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

函式命名不能用數字跟`-`開頭。<br>
以下都會產生錯誤。<br>
{% highlight python linenos %}
def 1test():
    print("test")
def -test():
    print("test")
{% endhighlight %}

## 函式傳回值
### 無return
```
def 函式名(參數1, 參數2...):
    程式碼
```
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

### 有return
```
def 函式名(參數1, 參數2...):
    return 程式碼
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

### 多個傳回值
{% highlight python linenos %}
def get_info(a, b):
    return a + b, a - b


r1, r2 = get_info(100, 20)
print(f"r1: {r1}, r2: {r2}")
{% endhighlight %}
```
r1: 120, r2: 80
```

## 函式全域變數 區域變數
函式內宣告的區域變數，函式之外不能使用。<br>
以下程式碼編譯失敗。<br>
{% highlight python linenos %}
def fun1():
    x = 50
    print(f"x = {x}")

print(f"x = {x}")
{% endhighlight %}

全域變數是n。<br>
fun1自己宣告區域變數n，跟全域變數n是二個獨立個體，不相同。<br>
不會修改到全域變數的n的內容。<br>
{% highlight python linenos %}
n = 100

def fun1():
    n = 50
    print(f"fun1() n = {n}")

fun1()
print(f"outside n = {n}")
{% endhighlight %}
```
fun1() n = 50
outside n = 100
```

如果要修改函式外部的全域變數，函式內要使用global。
{% highlight python linenos %}
n = 100

def fun1():
    global n  # 使用global 變數名
    n = 50
    print(f"fun1() n = {n}")

fun1()
print(f"outside n = {n}")
{% endhighlight %}
```
fun1() n = 50
outside n = 50
```
## 函式參數
### 函式參數傳遞 記憶體位址
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

1.先執行主程式`n=55`，在Stack堆疊區中，建立新的Stack，n的變數存的是0x0011，Memory中的Heap區，0x0011記憶體位址存的是55。<br>
![img]({{site.imgurl}}/python/stack1.png)<br>

2.呼叫test()函式。<br>
![img]({{site.imgurl}}/python/stack2.png)<br>

3.執行到test(a)函式後，在Stack堆疊區中，建立新的Stack，Stack建立方式由下至上。<br>
a的變數存的是0x0011。<br>

![img]({{site.imgurl}}/python/stack3.png)<br>

4.`a=10`，會把變數a存0x0022的記憶體位址，0x0022記憶體位址存的是10。<br>
![img]({{site.imgurl}}/python/stack4.png)<br>

5.test()函式執行完畢後，test函式的Stack會被清除，stack記憶體位址會被記憶體回收，供之後其它變數使用。<br>
![img]({{site.imgurl}}/python/stack5.png)<br>

6.主函式main()執行完畢後，主函式main()的Stack會被清除，stack記憶體位址會被記憶體回收，供之後其它變數使用。<br>
![img]({{site.imgurl}}/python/stack6.png)<br>

函式內的變數，是屬於區域變數，離開函式無法再讀取，因為函式的記憶體位址在離開函式時已被清除。<br>

### 相同的函式名
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

### 呼叫的函式參數不一致
Python函式沒有多型，以下程式碼，執行`test()`，它不會去配對參數的數量，Python會執行離自己最近的同名函式test(x)。<br>
就會產生參數數量無法配對的Error。<br>
{% highlight python linenos %}
def test():
    print("1")

test()
def test(x):
    print("2")

test()
{% endhighlight %}
```
TypeError: test() missing 1 required positional argument: 'x'
```

### 函式參數沒有類型
因為參數沒有類型，所以預期是字串的參數，呼叫函式時把參數設為int、float也是可以的。<br>
{% highlight python linenos %}
def get_info(name, price):
    print(f"name: {name}, price: {price}")

get_info(55, "Mary")
{% endhighlight %}
```
name: 55, price: Mary
```

### 參數預設值
可以為參數指派預設值，但有預設值的參數要放在後面。<br>
如果參數有預設值，呼叫函式時，可以不用傳入參數。<br>
{% highlight python linenos %}
def get_info(produt_name, price=10):
    print(f"produt_name = {produt_name}, price = {price}")


get_info("Cookie")
{% endhighlight %}
```
produt_name = Cookie, price = 10
```

以下會編譯失敗，因為有預設值的參數要放在後面，但在下面程式碼，卻放在前面。<br>
{% highlight python linenos %}
def get_info(produt_name = "Cookie", price):
    print(f"produt_name = {produt_name}, price = {price}")
{% endhighlight %} 

### 可變參數
參數數量不固定，可能有0個參數、1個參數，也可能有多個參數。<br>
參數的數量為0至多個參數，會把多個參數存入tuple。<br>

參數名使用`*`星號開頭，如下面程式碼，使用`*args`作為可變參數。<br>

語法  
```
def sum(*args):
    程式碼
```

以下程式碼，`{args}`直接輸出tuple的內容，使用`type()`輸出類型是tuple。<br>
{% highlight python linenos %}
def get_info(*args):
    print(f"args: {args} , type: {type(args)}")

get_info("Cookie", "Bread", "Fish")
{% endhighlight %}
```
args: ('Cookie', 'Bread', 'Fish') , type: <class 'tuple'>
```

使用for可以把tuple的元素一個一個輸出。
{% highlight python linenos %}
def get_info(*args):
    for arg in args:
        print(arg)

get_info("Cookie", "Bread", "Fish")
{% endhighlight %}
```
Cookie
Bread
Fish
```

使用return把傳回值傳入result的變數中。<br>
{% highlight python linenos %}
def sum(*args):
    total =0
    for arg in args:
        total += arg
    return total

result = sum(1,2,3,4,5,6,7,8,9)
print(result)
{% endhighlight %}

`*args`為0至多個參數，若無任何參數，也可以。<br>
以下程式碼呼叫sum()是不代入任何參數。<br>
{% highlight python linenos %}
def sum(*args):
    total =0
    for arg in args:
        total += arg
    return total

result = sum()
print(result)
{% endhighlight %}
```
0
```

### key與value的參數
參數的組成為key與value，此處key的命名仍要符合變數命名規範，不能數字開頭。<br>
```
呼叫函式(key1 = value1, key2 = value2, key3 = value3, key4 = value4)
info(name="Mary", age=18)
```

傳入的參數key與value會變成dict的資料型態。<br>

使用二個星星`**`代表參數是key與value。

語法
```
def info(**args):
	程式碼
```
{% highlight python linenos %}
def info(**args):
    print(f"args: {args}, type: {type(args)}")
    for key in args:
        print(f"{key}: {args[key]}")


info(name="Mary", age=18)
{% endhighlight %}
```
args: {'name': 'Mary', 'age': 18}, type: <class 'dict'>
name: Mary
age: 18
```

## 函式與字串Memory Model
Prerequisites:

- [字串][1]

1.在Stack建立main()的Stack，變數str1指向0x100的記憶體位址。<br>
![img]({{site.imgurl}}/python/str_stack1.png)<br>

2.進入change()函式，把str1的記憶體位址0x100「複製」給str_param變數，讓str_param也指向0x100。<br>
![img]({{site.imgurl}}/python/str_stack2.png)<br>

3.str_param變數指向0x200的記憶體位址。<br>
![img]({{site.imgurl}}/python/str_stack3.png)<br>

4.執行完change()函式後，change函式被記憶體回收，相關的stack與str_param變數也清除掉。<br>
main()函式中的str1仍是指向0x100的記憶體位址。<br>
![img]({{site.imgurl}}/python/str_stack4.png)<br>

不同函式之間的參數傳遞，是用「複製」記憶體位址的方式，把複製的記憶體位址指派給函式中的變數。<br>

完整程式碼:
{% highlight python linenos %}
def change(str_param):
    str_param += "abc"
    print(f"str_param: {str_param}")

str1 = "Hi"
change(str1)
print(f"str1: {str1}")
{% endhighlight %}
```
str_param: Hiabc
str1: Hi
```

[1]: {% link _pages/c/function/callByValue.md %}
[2]: {% link _pages/python/string.md %}


