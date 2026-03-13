---
title: string 字串
date: 2026-03-10
keywords: Python, string
---
## 型別為str
使用type()函式把string的型別輸出，輸出結果為str。<br>
{% highlight python linenos %}
str1 = "Hello"
print(f"type(str1) = {type(str1)}")
{% endhighlight %}
```
type(str1) = <class 'str'>
```

## 語法
### 使用雙引號`""`
{% highlight python linenos %}
str1 = "Hello"
print(f"str1 = {str1}")
{% endhighlight %}
```
str1 = Hello
```

### 使用單引號`''`
Python沒有字元(C++、Java有字元)，單引號包住的是字串。<br>
{% highlight python linenos %}
print(type('A'))
{% endhighlight %}
```
<class 'str'>
```

以下範例使用單引號\'\'<br>
{% highlight python linenos %}
str2 = 'Hello'
print(f"str2 = {str2}")
{% endhighlight %}
```
str2 = Hello
```

### 雙引號包住單引號 單引號包住雙引號
雙引號包住單引號
{% highlight python linenos %}
str3 = "Hi 'Amy'"
print(f"str3 = {str3}")
{% endhighlight %}
```
str3 = Hi 'Amy'
```

單引號包住雙引號
{% highlight python linenos %}
str4 = 'Hi "Amy"'
print(f"str4 = {str4}")
{% endhighlight %}
```
str4 = Hi "Amy"
```

### 三個雙引號與三個雙引號
使用三個雙引號\"\"\" 內容 \"\"\"
被三個雙引號包住的，裡面的格式都可以保留。
{% highlight python linenos %}
str5 = """
def max(a, b):
    if a > b:
        return a
    else:
        return b
"""
print(f"str5 = {str5}")
{% endhighlight %}
```
str5 = 
def max(a, b):
    if a > b:
        return a
    else:
        return b
```

使用三個單引號\'\'\'  內容   \'\'\'
被三個雙引號包住的，裡面的格式都可以保留。
{% highlight python linenos %}
str5 = '''
def max(a, b):
    if a > b:
        return a
    else:
        return b
'''
print(f"str5 = {str5}")
{% endhighlight %}
```
str5 = 
def max(a, b):
    if a > b:
        return a
    else:
        return b
```

## +號字串相連
雙引號與單引號都是字串，字串的型別，使用\+號，代表把二個字串連起來。<br>
{% highlight python linenos %}
str1 = "Hello" + 'A'
print(f"str1 = {str1}")
{% endhighlight %}
```
str1 = HelloA
```

## r與跳脫字元
`\n`與`\t`都是跳脫字元。<br>
在雙引號或單引號最前面加上r，字串中有`\n`就不會斷行，反而直接輸出`\n`。<br>
{% highlight python linenos %}
str1 = r"Hello\n World \t Hi"
print(f"str1 = {str1}")
{% endhighlight %}
```
str1 = Hello\n World \t Hi
```

## 字串Memory Layout

- [id字串駐留][1]

在[id][1]的文章中有討論到字串駐留。<br>

字串駐留的條件:<br>
- 字串是由大小寫英文字母、0-9、底線_
- 字串長度為0或1

產生字串「物件」會存到Heap中，Memory中stack是儲存變數，Memory中Heap是儲存物件。<br>

物件本身的記憶體位址是0x0100。<br>
物件中有記錄指向H的記憶體位址0x0011。<br>
物件中有記錄指向i的記憶體位址0x0022。<br>

![img]({{site.imgurl}}/python/str_memory.png)

以下程式碼顯示字串物件記憶體位址為4488577600。<br>
H的記憶體位址為4498451176。
i的記憶體位址為4498452760。
{% highlight python linenos %}
str1 = "Hi"
print(f"str1 = {str1}, id = {id(str1)}")
print(f"H address = {id(str1[0])}")
print(f"i address = {id(str1[1])}")
{% endhighlight %}
```
str1 = Hi, id = 4488577600
H address = 4498451176
i address = 4498452760
```

以下程式碼顯示，不同變數的內容都為Hi，實際上都指向相同字串物件記憶體位址4488577600。<br>
{% highlight python linenos %}
str1 = "Hi"
print(f"str1 = {str1}, id = {id(str1)}")
str2 = 'Hi'
print(f"str2 = {str2}, id = {id(str2)}")
str3 = """Hi"""
print(f"str3 = {str3}, id = {id(str3)}")
str4 = '''Hi'''
print(f"str4 = {str4}, id = {id(str4)}")
{% endhighlight %}
```
str1 = Hi, id = 4488577600
str2 = Hi, id = 4488577600
str3 = Hi, id = 4488577600
str4 = Hi, id = 4488577600
```
## 編碼
### ord()
字串中每個單字是由Unicode組成，Python提供ord()可以查詢Unicode碼。<br>
參數可以是雙單號，也可以是單引號，二者皆為字串。<br>
{% highlight python linenos %}
print(ord('A'))
print(ord("A"))
{% endhighlight %}
```
65
65
```

### chr()
透過 ASCII 或 Unicode 得到對應的字元。<br>
{% highlight python linenos %}
print(chr(65))
print(chr(97))
print(chr(48))
{% endhighlight %}
```
A
a
0
```

### 字串比較
字串可以比較，比較的運算子有 > < >= <= == !=

會根據ord()函式一個一個字母比對大小。<br>
{% highlight python linenos %}
str1 = "abc"
str2 = "ab"
print(str1 < str2)
{% endhighlight %}
```
False
```

## [索引]取出字串中的字元
使用`[索引]`取出字串中的單字，語法:<br>
```
字串變數[索引]
```

{% highlight python linenos %}
str1 = "Hello"
print(str1[0])
print(str1[1])
print(str1[2])
{% endhighlight %}
```
H
e
l
```

Python沒有字元，透過索引取出來的單字，型別仍是字串。
{% highlight python linenos %}
str1 = "Hello"
print(f"str1[0] = {str1[0]} , type = {type(str1[0])}")
{% endhighlight %}
```
str1[0] = H , type = <class 'str'>
```

### 無法透過索引修改字串的單字
以下程式碼會編譯錯誤。<br>
{% highlight python linenos %}
str1 = "Hello"
st1[0] = "A"
{% endhighlight %}

## 字串遍歷
### for
{% highlight python linenos %}
str1 = "Hello"
for elem in str1:
    print(elem)
{% endhighlight %}
```
H
e
l
l
o
```

### while
{% highlight python linenos %}
str1 = "Hello"
index = 0
while index < len(str1):
    print(str1[index])
    index += 1
{% endhighlight %}

## 負數索引
-1為字串最後一位，-2為倒數第2位。<br>

|字串   | H| e| l| l| o|
|:---:|:---:|:---:|:---:|:---:|:---:|
|正數索引| 0| 1| 2| 3| 4|
|負數索引| -5| -4| -3| -2| -1|

{% highlight python linenos %}
str1 = "Hello World"
print(str1[-1])
print(str1[-2])
print(str1[-3])
{% endhighlight %}
```
d
l
r
```

## 字串相關函式
### str.capitalize() 第一個字母大寫
{% highlight python linenos %}
str1 = "hello"
print(str1.capitalize())
{% endhighlight %}
```
Hello
```

### str.upper() 全部大寫
{% highlight python linenos %}
str1 = "hello"
print(str1.upper())
{% endhighlight %}

### str.lower() 全部小寫
{% highlight python linenos %}
str1 = "hello"
print(str1.lower())
{% endhighlight %}
```
hello
```

### len()字串長度
{% highlight python linenos %}
str1 = "hello"
print(len(str1))
{% endhighlight %}
```
5
```

### str.count()字串出現次數
單字出現次數 
{% highlight python linenos %}
str1 = "hello"
print(str1.count("l"))
{% endhighlight %}
```
2
```

字串出現次數
{% highlight python linenos %}
str1 = "Hello world world"
print(str1.count("world"))
{% endhighlight %}
```
2
```
### str1.index() 尋找字串所在索引位置
尋找單字
{% highlight python linenos %}
str1 = "hello"
print(str1.index("l"))
{% endhighlight %}
```
2
```

尋找字串第一個字母所在位置。<br>
{% highlight python linenos %}
str1 = "Hello world world"
print(str1.index("world"))
{% endhighlight %}
```
6
```
### str1.replace() 取代字串
語法
```
str.replace("舊文字","新文字",取代幾次)
```

如果第3個參數不寫，預取是取代所有的l。
{% highlight python linenos %}
str1 = "hello"
print(str1.replace("l","A"))
{% endhighlight %}
```
heAAo
```

如果第3個參數有寫，以下程式碼是取代1個l。
{% highlight python linenos %}
str1 = "hello"
print(str1.replace("l","A",1))
{% endhighlight %}
```
heAlo
```

取代字串。
{% highlight python linenos %}
str1 = "Hello world world"
print(str1.replace("world", "Mary"))
{% endhighlight %}
```
Hello Mary Mary
```

### str1.strip()去除前後字元
注意！只會去除前後字元，以下程式碼去掉字串前的空白與後面的空白。<br>
{% highlight python linenos %}
str1 = "   Hello world    "
print(str1.strip(" "))
{% endhighlight %}
```
Hello world
```

以下程式碼去除字串前後的abc字串。
{% highlight python linenos %}
str1 = "abcHello abc worldabc"
print(str1.strip("abc"))
{% endhighlight %}
```
Hello abc world
```

### str.split()
根據空格或逗號切割字串，傳回值為list類型
{% highlight python linenos %}
str1 = "Hello World"
result = str1.split(" ")
print(f"result = {result}, type = {type(result)}")
{% endhighlight %}
```
result = ['Hello', 'World'], type = <class 'list'>
```

### str.isalpha() 是否為大小寫字母
以下程式碼判斷若為大小寫字母，把它變為大寫。<br>
{% highlight python linenos %}
str1 = "hello 123"
alpha = ""
for elem in str1:
    if elem.isalpha():
        alpha += elem.upper() + ", "
print(alpha)
{% endhighlight %}
```
H, E, L, L, O, 
```

[1]: {% link _pages/python/id_interning.md %}

