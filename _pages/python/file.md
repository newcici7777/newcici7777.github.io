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
注意，寫入時要加入`\n`，不然會全連在一起，不會有斷行。<br>
{% highlight python linenos %}
f = open("/Users/cici/testc/file_test3", "w", encoding="utf-8")
f.write("Hello\n")
f.write("Nice to meet you! \n")
f.close()
{% endhighlight %}

使用迴圈寫入。<br>
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

## 檔案
要先匯入os 套件。<br>
```
import os
```
### 判斷檔案是否存在 os.path.exists()
{% highlight python linenos %}
import os
if os.path.exists("/Users/cici/testc/file_test2"):
    print("存在")
else:
    print("不存在")
{% endhighlight %}

### 刪除檔案 os.remove()
{% highlight python linenos %}
import os
if os.path.exists("/Users/cici/testc/file_test3"):
    os.remove("/Users/cici/testc/file_test3")
else:
    print("不存在")
{% endhighlight %}

## 目錄
### 判斷目錄是否存在 os.path.isdir()
{% highlight python linenos %}
import os
if os.path.isdir("/Users/cici/testc/dir1"):
    print("dir1 exists")
else:
    print("dir1 not exists")
{% endhighlight %}

### 建立一個目錄 os.mkdir()
以下範例， `/Users/cici/testc/` 是確實存在的目錄，但dir1是不存在的目錄，以下範例只建立一個目錄。<br>
{% highlight python linenos %}
import os
if os.path.isdir("/Users/cici/testc/dir1"):
    print("dir1 exists")
else:
    os.mkdir("/Users/cici/testc/dir1")
{% endhighlight %}

### 建立多個子目錄 os.makedirs()
以下程式碼試圖在 dir1 目錄下建立 dir2 目錄，在 dir2 目錄下建立 dir3 目錄。<br>
會產生 FileNotFoundError。
{% highlight python linenos %}
import os
if os.path.isdir("/Users/cici/testc/dir1/dir2/dir3"):
    print("dir3 exists")
else:
    os.mkdir("/Users/cici/testc/dir1/dir2/dir3")
{% endhighlight %}
```
FileNotFoundError: [Errno 2] No such file or directory: '/Users/cici/testc/dir1/dir2/dir3'
```

要一次建立多個子目錄，使用 os.makedirs()，方法最後面有加個s。
{% highlight python linenos %}
import os
if os.path.isdir("/Users/cici/testc/dir1/dir2/dir3"):
    print("dir3 exists")
else:
    os.makedirs("/Users/cici/testc/dir1/dir2/dir3")
{% endhighlight %}

### 刪除目錄 os.rmdir()
若目錄沒有子目錄、也沒有檔案，就可以使用 os.rmdir() 刪除目錄。<br>
{% highlight python linenos %}
# /Users/cici/testc/file_test2
import os
if os.path.isdir("/Users/cici/testc/dir1/dir2/dir3"):
    os.rmdir(
        "/Users/cici/testc/dir1/dir2/dir3")
else:
    print("dir3 not exists")
{% endhighlight %}

### 刪除父目錄 os.removedirs()
以下範例，dir2下面只有dir3，dir3 刪完後，會去刪它的父目錄 dir2 ，如果dir2 目錄沒有任何檔案、目錄。
{% highlight python linenos %}
import os
if os.path.isdir("/Users/cici/testc/dir1/dir2/dir3"):
    os.removedirs("/Users/cici/testc/dir1/dir2/dir3")
else:
    print("dir3 not exists")
{% endhighlight %}

## 檔案大小 建立時間
{% highlight python linenos %}
import os
import time
f_stat = os.stat("/Users/cici/testc/")
print(f"size : {f_stat.st_size} \n"
      f"建立時間: {time.ctime(f_stat.st_ctime)} \n"
      f"修改時間: {time.ctime(f_stat.st_mtime)} \n"
      f"最近開啟時間: {time.ctime(f_stat.st_atime)} \n")
{% endhighlight %}
```
size : 640 
建立時間: Mon Jun  1 12:27:08 2026 
修改時間: Mon Jun  1 12:27:08 2026 
最近開啟時間: Mon Jun  1 12:27:09 2026 
```

## f.flush() 強制立刻寫入
- [sleep ctime][1]

寫入時並非立刻寫入，會先存在緩衝區，直到檔案close()，才會寫入。<br>

sleep(秒數)，以下程式碼，在10秒內打開檔案都是沒有內容。<br>
{% highlight python linenos %}
import time
f = open("/Users/cici/testc/file_test3", "w", encoding="utf-8")
f.write("Hello\n")
f.write("Nice to meet you! \n")
print(f"開始等待 {time.ctime()}")
time.sleep(10)
print(f"結束等待 {time.ctime()}")
f.close()
{% endhighlight %}
```
開始等待 Mon Jun  1 13:08:17 2026
結束等待 Mon Jun  1 13:08:27 2026
```

使用 flush() 強制立刻將緩衝區寫入至檔案。<br>
{% highlight python linenos %}
# /Users/cici/testc/file_test2
import time
f = open("/Users/cici/testc/file_test3", "w", encoding="utf-8")
f.write("Hello\n")
f.write("Nice to meet you! \n")
f.flush()
print(f"開始等待 {time.ctime()}")
time.sleep(10)
print(f"結束等待 {time.ctime()}")
f.close()
{% endhighlight %}
```
開始等待 Mon Jun  1 13:14:39 2026
結束等待 Mon Jun  1 13:14:49 2026
```

## f.close() 
f.close() 本身就是flush() 將緩衝區的內容寫入至檔案，與關閉檔案二個功能。

## with open()
使用with open() 會自動關閉檔案，不用手動close()檔案。

語法:
```
with open("檔案路徑", mode) as f:
```` 
以上的語法會產生 f 物件，並可以對 f物件進行相關讀檔案等操作。

{% highlight python linenos %}
with open("/Users/cici/testc/file_test2", "r") as f:
    lines = f.readlines()
    for line in lines:
        print(line, end="")

print(f"檔案是否關閉？",f.closed)
{% endhighlight %}
```
Hello!
Hi!
Thank you.
Nice to see you.檔案是否關閉？ True
```

## 拷貝圖檔 BinaryFile
圖檔是 BinaryFile，所以 mode 會使用到`b`。
{% highlight python linenos %}
# 來源檔路徑
src_path = "/Users/cici/testc/1.png"
# 目標檔路徑
dest_path = "/Users/cici/testc/copy.png"
# 讀取檔案用rb，b代表 BinaryFile
fsrc = open(src_path, "rb")
# 讀取資料
data = fsrc.read()
# 寫入檔案用wb，b代表 BinaryFile
fdest = open(dest_path, "wb")
fdest.write(data)
fdest.close()
fsrc.close()
{% endhighlight %}

以下程式碼會自動關閉檔案:<br>
{% highlight python linenos %}
src_path = "/Users/cici/testc/1.png"
dest_path = "/Users/cici/testc/copy.png"
with open(src_path, "rb") as fsrc:
    with open(dest_path, "wb") as fdest:
        for data in fsrc:
            fdest.write(data)
{% endhighlight %}

## 列出目錄中所有檔案 os.listdir()
{% highlight python linenos %}
import os
path = "/Users/cici/testc"
content = os.listdir(path)
print(content)
{% endhighlight %}
```
['testc.c', '.DS_Store', 'libTest.a', 'test', 'copy1.png', 'print_out', '401', 'file_test2', 'test.o', 'file_test3', 'file_test', 'testc.cpp', 'testc.o', 'libTest.so', 'dir1', 'testc', '2.png', '3.png', '1.png', 'copy.png']
```

{% highlight python linenos %}
import os
path = "/Users/cici/"
content = os.listdir(path)
for ele in content:
    child = path + "/" + ele
    if os.path.isdir(child):
        print(f"目錄: {child}")
    else:
        print(f"檔案: {child}")
{% endhighlight %}
```
檔案: /Users/cici//Dog.class
目錄: /Users/cici//.eclipse
目錄: /Users/cici//.config
目錄: /Users/cici//Music
目錄: /Users/cici//vtable_test
檔案: /Users/cici//.zprofile.pysave
目錄: /Users/cici//ffmpeg
目錄: /Users/cici//.skiko
目錄: /Users/cici//devops2
目錄: /Users/cici//.trae
```

遞迴，以下仍待修正:
{% highlight python linenos %}
import os
def print_info(dir_path):
    content = os.listdir(dir_path)
    for ele in content:
        child = path + "/" + ele
        if os.path.isdir(child):
            print(f"目錄: {child}")
            print_info(child)
        else:
            print(f"檔案: {child}")

path = "/Users/cici/testc"
print_info(path)
{% endhighlight %}

[1]: {% link _pages/python/sleep_ctime.md %}