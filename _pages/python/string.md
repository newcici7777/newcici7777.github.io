---
title: string 字串
date: 2026-03-10
keywords: Python, string
---
## 型別為str
使用type把string的型別輸出，輸出結果為str。<br>
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

[1]: {% link _pages/python/id.md %}

