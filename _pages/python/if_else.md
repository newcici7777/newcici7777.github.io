---
title: if else
date: 2026-02-26
keywords: Python, if else
---
## 沒有三元運算
Python沒有`?:`三元運算，取而代之如下:
```
max = a if a > b else b
```
如果 a > b，傳回a。<br>
否則，傳回b。<br>

![img]({{site.imgurl}}/python/if_else1.png)<br>

{% highlight python linenos %}
a = 5
b = 100
max = a if a > b else b
print("max = ", max)
min = a if a < b else b
print("min = ", min)
{% endhighlight %}
```
max =  100
min =  5
```

## if
語法
```
if 條件:
	程式碼1
	程式碼2

	程式碼3
```
Python 的 if 沒有一對花括號`{}`包住，使用Tab或空格縮排，區分`程式碼區塊`。<br>
以下雖然有空一行，但前方的空格數量是一致，屬於同一個區塊。<br>
{% highlight python linenos %}
if 5 < 8 :
    print("hello")

    print("world")
print("ABC")
{% endhighlight %}
```
hello
world
ABC
```

即便空格很多，若前方空格數量是一致，屬於同一個區塊。<br>
{% highlight python linenos %}
if 5 < 8 :
            print("hello")
        
            print("world")
print("ABC")
{% endhighlight %}
### 沒有空格會有錯誤
以下程式碼，if條件中的程式碼區塊，左邊沒有任何空格，即便1個空格也可以，如果沒有任何空格，執行會有錯誤。
{% highlight python linenos %}
a = 10
b = 20
c = 30
if a < b:
print("a < b")
if a > b:
print("a > b")
{% endhighlight %}
```
    print("a < b")
    ^^^^^
IndentationError: expected an indented block after 'if' statement on line 4
```

### if 包 if
相同的縮排，屬於同一個程式碼區塊。<br>
{% highlight python linenos %}
a = 10
b = 20
c = 30
if a < b:
    if a < c:
        print("a < c")
    print("a < b")
{% endhighlight %}
```
a < c
a < b
```

### elif else
其它程式語言Java,C++是`else if`，Python是`elif`。<br>

條件為True，才執行程式碼區塊。

語法，(無else)
```
if 條件:
  程式碼區塊
elif 條件:
  程式碼區塊
```

語法，有else
```
if 條件:
  程式碼區塊
elif 條件:
  程式碼區塊
else:
  程式碼區塊
```

多個條件中，只選其中`一個`條件，條件中的程式碼區塊執行完了，就離開整個if條件。<br>

當多個elif都不符合條件，就會執行else，注意！else非必要存在，即便沒有else，只有if ... elif 也可以。<br>

以下程式碼，所有條件，只會執行其中`一個`條件，然後就離開整個if條件。<br>
不會執行 >= 90 的程式碼區塊後，又去執行 >= 80 的程式碼區塊。<br>

多個條件，只選擇一個條件。<br>
{% highlight python linenos %}
score = float(input("請輸入成績"))
if score >= 90:
    print("優")
elif score >= 80:
    print("甲")
elif score >= 70:
    print("乙")
elif score >= 60:
    print("丙")
else:
    print("丁")
{% endhighlight %}
```
請輸入成績75
乙
```

以下程式碼，即便沒有else也能正常執行。<br>
{% highlight python linenos %}
score = float(input("請輸入成績"))
if score >= 90:
    print("優")
elif score >= 80:
    print("甲")
elif score >= 70:
    print("乙")
elif score >= 60:
    print("丙")
{% endhighlight %}
```
請輸入成績50
```

以下程式碼只會執行一句，程式碼縮排決定層次。<br>
{% highlight python linenos %}
score = 85
if score >= 80:
    if score >= 90:
        print("優秀")  # 不會執行
    print("做的好")
else:
    print("再加油")  # 不會執行
{% endhighlight %}
```
做的好
```

### 條件使用圓括號
潤年條件有以下二個:
1. 西元年可被4整除 並且 西元年除100餘數不為0
2. 西元年可被400整除

以下程式碼使用圓括號把`2`個條件區分開來。
{% highlight python linenos %}
year = 2024
if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0:
    print(f"{year}是潤年")
else:
    print(f"{year}不是潤年")
{% endhighlight %}
```
2024是潤年
```

### 0 <= x <= 100
if可以使用以下條件語句，x介於 0 至 100之間，0跟100是可以替換。
```
0 <= x <= 100
0 < x < 100
0 <= x < 100
0 < x <= 100
```

{% highlight python linenos %}
score = 55
if 90 <= score <= 100 :
    print(f" {score} 優")
elif 80 <= score < 90:
    print(f" {score} 甲")
elif 70 <= score < 80:
    print(f" {score} 乙")
elif 60 <= score < 70:
    print(f" {score} 丙")
elif 0 <= score < 60:
    print(f" {score} 丁")
{% endhighlight %}
```
 55 丁
```

{% highlight python linenos %}
month = int(input("請輸入月份:"))
if 3 <= month <= 5:
    print(f" {month} 月是春天")
elif 6 <= month <= 8:
    print(f" {month} 月是夏天")
elif 9 <= month <= 11:
    print(f" {month} 月是秋天")
elif 12 <= month <= 2:
    print(f" {month} 月是冬天")
else:
    print("你的月份輸入錯誤5")
{% endhighlight %}
```
請輸入月份:5
 5 月是春天
```