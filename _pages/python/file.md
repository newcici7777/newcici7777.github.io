---
title: File
date: 2026-06-01
keywords: Python, File
---
## open()
open() 語法
```
file_obj = open("/目錄/檔名", mode, encoding)
```

mode:
- r 讀取(預設)
- w 建立檔案(不管是否有存在)，若檔案有存在，清空檔案內容。
- x 若檔案有存在，則不建立，若檔案不存在，則建立。
- a 寫入的內容附加在檔案末尾
- b binary file 開啟
- t text file 開啟

encoding:
- utf8 (預設)
- big5

傳回值:file_obj<br>

參考文件:<https://docs.python.org/zh-cn/3.12/library/functions.html#open><br>

### mode = w
建立檔案，若原本就有檔案，則清空檔案內容。<br>
{% highlight python linenos %}
f = open("/目錄/檔名", "w", encoding="utf-8")
print(type(f))
{% endhighlight %}
```
<class '_io.TextIOWrapper'>
```

### mode = x
若檔案已存在就會產生 FileExistsError 。
{% highlight python linenos %}
f = open("/目錄/檔名", "x", encoding="utf-8")
{% endhighlight %}
```
FileExistsError: [Errno 17] File exists: '/目錄/檔名'
```

### mode = r
讀取檔案
{% highlight python linenos %}
f = open("/目錄/檔名", "r", encoding="utf-8")
{% endhighlight %}

## f.read()
讀取所有內容。<br>
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test2", "r", encoding="utf-8")
content = f.read()
f.close()
print(content)
{% endhighlight %}
```
Hello!
Hi!
Thank you.
Nice to see you.
```

## f.read(byte)
byte為讀取內容的大小。<br>
以下範例為讀取3個英文字元。<br>
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test2", "r", encoding="utf-8")
content = f.read(3)
f.close()
print(content)
{% endhighlight %}
```
Hel
```

## 讀取檔案
### f.readline() 讀一行，並保留斷行
檔案內容如下，每一行最後面都會有一個隱藏`\n`。
```
Hello!
Hi!
Thank you.
Nice to see you.
```

以下範例，讀取每一行，都會讀取到`\n`，因此執行結果每讀一行，就會斷行。
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test2", "r", encoding="utf-8")
line1 = f.readline()
line2 = f.readline()
print(f"line1 = {line1}")
print(f"line2 = {line2}")
{% endhighlight %}
```
line1 = Hello!

line2 = Hi!

```

### f.readline() 結合無窮迴圈
以下的程式碼，代表已經沒有讀取到任何內容，則離開無窮迴圈。
```
if line == "":
    break
```

{% highlight python linenos %}
f = open("/Users/cici/testc/file_test2", "r", encoding="utf-8")
while True:
    line = f.readline()
    if line == "":
        break
    print(line, end="")  # end = "" 代表不要斷行
f.close()
{% endhighlight %}
```
Hello!
Hi!
Thank you.
Nice to see you.
```

### f.readlines()
f.readlines() 傳回值類型為list。<br>
注意！list中每一個元素，代表每一行，後面都有`\n`。
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test2", "r", encoding="utf-8")
lines = f.readlines()
print(type(lines))
print(lines)
{% endhighlight %}
```
<class 'list'>
['Hello!\n', 'Hi!\n', 'Thank you.\n', 'Nice to see you.']
```

使用for 讀取 list。
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test2", "r", encoding="utf-8")
lines = f.readlines()
for line in lines:
    print(line, end = "")
f.close()
{% endhighlight %}
```
Hello!
Hi!
Thank you.
Nice to see you.
```

### 對檔案物件 使用迴圈
每讀一次f 物件，就取一行。
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test2", "r", encoding="utf-8")
# 每讀一次f 物件，就取一行
for line in f:
    print(line, end="")
f.close()
{% endhighlight %}
```
Hello!
Hi!
Thank you.
Nice to see you.
```

## 寫入檔案
### mode = w
以下程式碼建立檔案，如果沒有此檔案，則會建立，若已經有此檔案，則會清空內容，並寫入新的內容。<br>
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test3", "w", encoding="utf-8")
i = 1
while i <= 10:
    f.write(f"Hello {i} !\n")
    i += 1
f.close()
{% endhighlight %}
檔案內容
```
Hello 1 !
Hello 2 !
Hello 3 !
Hello 4 !
Hello 5 !
Hello 6 !
Hello 7 !
Hello 8 !
Hello 9 !
Hello 10 !
```

### mode = a
若寫入模式改成a，就會把新內容附加在原本檔案內容後面。<br>
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test3", "a", encoding="utf-8")
i = 1
while i <= 10:
    f.write(f"append Hello {i} !\n")
    i += 1
f.close()
{% endhighlight %}
檔案內容
```
Hello 1 !
Hello 2 !
Hello 3 !
Hello 4 !
Hello 5 !
Hello 6 !
Hello 7 !
Hello 8 !
Hello 9 !
Hello 10 !
append Hello 1 !
append Hello 2 !
append Hello 3 !
append Hello 4 !
append Hello 5 !
append Hello 6 !
append Hello 7 !
append Hello 8 !
append Hello 9 !
append Hello 10 !
```

## os
要先匯入os 套件。<br>
```
import os
```
### 判斷檔案是否存在
{% highlight python linenos %}
import os
if os.path.exists("/Users/cici/testc/file_test2"):
    print("存在")
else:
    print("不存在")
{% endhighlight %}