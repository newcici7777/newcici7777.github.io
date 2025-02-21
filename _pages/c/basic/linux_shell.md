---
title: Linux shell指令
date: 2025-02-13
keywords: linux, shell
---

## 查看檔案

### less分頁

```
less 檔名.副檔名
```
1. q:離開
2. 空格: 下一頁
3. b: 上一頁
4. Enter與j: 往下移動一行
5. k: 往上移動一行
6. g：跳到文件的開頭。
7. G：跳到文件的結尾

### more分頁

```
more 檔名.副檔名
```
1. q:離開
2. 空格: 下一頁
3. b: 上一頁
4. Enter與j: 往下移動一行
5. k: 往上移動一行
6. g：跳到文件的開頭。
7. G：跳到文件的結尾

### wc統計行數

```
wc 檔名.副檔名
```

```
% wc config.h
     784    2369   22076 config.h
```
- 784 行數
- 2369 字數
- 22076byte 檔案大小(單位是byte)


統計目前目錄所有檔案的行數,字數,檔案大小
```
wc *
```

統計多個檔案
```
wc 檔案1 檔案2 檔案3
```

### grep

以下指令為，在ffmpeg.txt中搜尋"input"
```
grep "input" ffmpeg.txt
```

在所有的head檔中，搜尋那個檔案中有包含"stdio.h"
```
grep "stdio.h" *.h
```

執行結果
```
common.h:#include <stdio.h>
file_open.h:#include <stdio.h>
internal.h:#include <stdio.h>
vulkan_loader.h:#include <stdio.h>
```
顯示的意思為

檔名.副檔名: 有出現 stdio.h 的那一行

### head
顯示檔案的前10筆
```
head 檔名.副檔名
```

顯示檔案的前15筆
```
head -n 15 檔名.副檔名
```

### tail
顯示檔案的後10筆
```
tail 檔名.副檔名
```

顯示檔案的後15筆
```
tail -n 15 檔名.副檔名
```

若檔案有新增筆數(例如0.1秒新增一筆)，使用以下指令在檔案有變更時會立即更新顯示結果
```
tail -f 檔名.副檔名
```
ctrl+c放棄追蹤更新

### ls

以下指令，最新的檔案排在最前面
```
ls -lt
```

### piple

若目錄下檔案很多，使用分頁功能查看。以下指令把ls的結果作為輸出給less指令去分頁
```
ls -lt | less
```

查看log，使用以下指令會列出檔案中2025-01-01的log，並以分頁方式顯示
```
grep "2025-01-01" 檔案.log | less
```

查看行數、字數、大小
```
grep "2025-01-01" 檔案.log | wc
```

## top查看系統效能
每3秒更新cpu系統效能畫面，ctrl+c離開
```
top
```

```
Load Avg: 3.93, 4.09, 3.47  
```
Load Avg:在一段時間內，cpu正在處理或等待處理的程序總合平均值，分別以最近1分鐘、5分鐘、15分鐘顯示

```
Processes: 543 total, 2 running, 541 sleeping, 2150 threads 
```
共有543程序，2個正在執行，541個在休眠，2150個執行緒

## env環境變數

顯示所有環境變數
```
env
```

使用grep搜尋要看的環境變數
```
env | grep PATH
```

環境變數分頁
```
env | less
```

使用echo顯示環境變數的值，注意！環境變數前要加錢字號$
```
echo $PATH
```

### 常用環境變數

#### LANG系統語言

預設中文是UTF-8
```
 % echo $LANG
en_US.UTF-8
```

#### PATH
PATH的值是多個目錄，包含linux bash指令存放位置，或user的應用程式的執行檔案存放位置，以:分號分隔目錄。

注意！環境變數前要加錢字號$
```
echo $PATH
/usr/local/Cellar/ruby/3.3.0/bin:/usr/bin:/usr/sbin:/bin:/sbin:/usr/X11R6/bin:/bin:/usr/bin:/usr/local/bin:/sbin:
```

例如ls指令是放在/usr/bin目錄下，如果PATH少了/usr/bin，就不能使用ls、wc、less...等等bash指令。

#### LD_LIBRARY_PATH
c++動態庫的目錄

## export

注意！export的效果只有在目前的shell中適用，重新登入就清掉了，需要設置系統環境變數才能永久有效。

方式一

```
變數=\'值\'
export 變數
```

```
TEST='TEST1 TEST2 TEST3'
export TEST
env | grep TEST
```

方式二
```
export 變數=\'值\'
```
如果值不是空格或特殊符號，可以不用\'\'

### 設定PATH
如果要增加/Users/cici/test目錄在PATH，可以用以下指令設定，$PATH代表原來PATH變數的值，再加上:冒號，後面加上要增加的目錄
```
export PATH=$PATH:/Users/cici/NDK
```

### 執行檔案

我增加/Users/cici/testc的目錄，在目錄中，增加testc.cpp的檔案，並且編譯產生執行檔testc

```
vi testc.cpp
g++ -o testc testc.cpp
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  cout << "Hello World!" << endl;
}
{% endhighlight %}

使用`./`來執行檔案，`.`圓點代表目前的目錄下，'./testc'代表目前目錄下的testc檔案，如果只寫`testc`是找不到目前目錄下的testc檔案
```
./testc
```

如果想在任何目錄都可以執行目錄目錄的執行檔案，可以在PATH增加.圓點，就可以執行testc
```
export PATH=$PATH:.
testc
```

以相對/絕對路徑執行指令，例如"/bin/ls"或"./ls"

## 設置使用者環境變數

### bash_profile
在user目錄下會有.bash_profile的檔案
```
cd ~
ls -l .bash*
-rw-r--r--@ 1 cici  staff  536 Aug 30 12:15 .bash_profile
```

使用cat指令查看檔案
```
 cat .bash_profile

# Setting PATH for Python 3.7
# The original version is saved in .bash_profile.pysave
PATH="/Library/Frameworks/Python.framework/Versions/3.7/bin:${PATH}"
export PATH

alias python='python3.7'
alias pip='pip3.7'

export ANDROID_HOME=/Users/cici/Library/Android/sdk
export PATH=${PATH}:${ANDROID_HOME}/tools
export PATH=${PATH}:${ANDROID_HOME}/platform-tools
export PATH=${PATH}:/Users/cici/.rbenv/versions/3.2.2/bin


# Add Visual Studio Code (code)
export PATH="\$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"
```
### bashrc
每開一個終端機，就會重新load一次bashrc