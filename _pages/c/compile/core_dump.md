---
title: gdb core dump
date: 2024-12-04
keywords: c++, gdb, core dump
---

產生memory leak的錯誤會存在core檔案。

linux預設不會產生core檔案，需修改系統參數。

## 查看core檔案大小語法

 ulimit是限制使用者資源使用量

ulimit -a
```
cici@cici-vm:~/test/app$ ulimit -a
real-time non-blocking time  (microseconds, -R) unlimited
core file size              (blocks, -c) 0
data seg size               (kbytes, -d) unlimited
scheduling priority                 (-e) 0
file size                   (blocks, -f) unlimited
pending signals                     (-i) 15194
max locked memory           (kbytes, -l) 495132
max memory size             (kbytes, -m) unlimited
open files                          (-n) 1024
pipe size                (512 bytes, -p) 8
POSIX message queues         (bytes, -q) 819200
real-time priority                  (-r) 0
stack size                  (kbytes, -s) 8192
cpu time                   (seconds, -t) unlimited
max user processes                  (-u) 15194
virtual memory              (kbytes, -v) unlimited
file locks                          (-x) unlimited
```
由以上列表，可以發現core file size = 0
<pre>
core file size              (blocks, <span class="markline">-c</span>) 0
</pre>
從以上列表，也可以看到stack大小
```
stack size                  (kbytes, -s) 8192
```

## 修改core檔案大小

-c是上述列表中有列出來的，-c代表的是core file size

將core file size設成無限unlimited
<pre>
$ ulimit -c unlimited
$ ulimit -a
real-time non-blocking time  (microseconds, -R) unlimited
<span class="markline">core file size              (blocks, -c) unlimited</span>
data seg size               (kbytes, -d) unlimited
scheduling priority                 (-e) 0
file size                   (blocks, -f) unlimited
pending signals                     (-i) 15194
max locked memory           (kbytes, -l) 495132
max memory size             (kbytes, -m) unlimited
open files                          (-n) 1024
pipe size                (512 bytes, -p) 8
POSIX message queues         (bytes, -q) 819200
real-time priority                  (-r) 0
stack size                  (kbytes, -s) 8192
cpu time                   (seconds, -t) unlimited
max user processes                  (-u) 15194
virtual memory              (kbytes, -v) unlimited
file locks                          (-x) unlimited
</pre>

## core dump

建立一個cpp檔案core_test1.cpp

以下func2()中，試圖給ptr指標設值，但ptr指標是nullptr。
{% highlight c++ linenos %}
#include <cstring>
#include <iostream>
using namespace std;
void func2(const string msg) {
  // 宣告nullptr的ptr指標
  char * ptr = nullptr;
  // 試圖給nullptr設值
  *ptr = 97;  // 97代表a
}
void func1(const string msg) {
  func2(msg);
}
int main() {
  func1("test");
  return 0;
}
{% endhighlight %}

執行以下指令

```
$ g++ -o demo04 core_test1.cpp -g
$ ./demo04
程式記憶體區段錯誤 (核心已傾印
$ ls
```

若是沒有產生\"core\"開頭的檔案，把core存放路徑改為目前執行程式的目錄下

```
echo "core" | sudo tee /proc/sys/kernel/core_pattern
```

再次執行程式

```
$ ./demo04
$ ls
```

檢查是否已經產生\"core\"開頭的檔案

## debug

輸入以下指令，core是先前步驟的檔名，每個人的檔名不同，都是以core開頭
```
gdb ./demo04 core
```

黃色是要輸入的字母
<pre>
Enable debuginfod for this session? (y or [n]) <span class="markline">y</span>
Debuginfod has been enabled.
To make this setting permanent, add 'set debuginfod enabled on' to .gdbinit.
Downloading separate debug info for system-supplied DSO at 0x7fffff0d2000
--Type < RET > for more, q to quit, c to continue without paging--<span class="markline">c</span>
[Thread debugging using libthread_db enabled]                                   
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Core was generated by `./demo04'.
Program terminated with signal SIGSEGV, Segmentation fault.
#0  0x00005a0b1c65d321 in func2 (msg=...) at core_test1.cpp:8
<span class="markline">8	  *ptr = 97;  // 97代表a</span>
(gdb) 
</pre>

```
8	  *ptr = 97;  // 97代表a
```
會告知你錯誤的地方在第8行

## bt

輸入bt，會列出呼叫函式的堆疊(call stack)

由下往上呼叫，最下面是main()呼叫func1()，func1()再呼叫func2()，最上層的是func2()

```
(gdb) bt
#0  0x00005a0b1c65d321 in func2 (msg="test") at core_test1.cpp:8
#1  0x00005a0b1c65d365 in func1 (msg="test") at core_test1.cpp:11
#2  0x00005a0b1c65d3d4 in main () at core_test1.cpp:14
```

## q

輸入q就會離開gdb

## 顯示錯誤的行號在c++函式庫

修改程式碼

{% highlight c++ linenos %}
#include <cstring>
#include <iostream>
using namespace std;
void func2(const string msg) {
  // 宣告nullptr的ptr指標
  char * ptr = nullptr;
  // 試圖給nullptr設值
  strcpy(ptr, msg.c_str());
}
void func1(const string msg) {
  func2(msg);
}
int main() {
  func1("test");
  return 0;
}
{% endhighlight %}

執行以下指令
```
$ g++ -o demo04 core_test1.cpp -g
$ ./demo04
程式記憶體區段錯誤 (核心已傾印)
$ gdb ./demo04 core
```

以下的錯誤無法看出來是什麼問題
```
#0  __strcpy_avx2 () at ../sysdeps/x86_64/multiarch/strcpy-avx2.S:127
```

使用bt可以看出錯誤
```
(gdb) bt
#0  __strcpy_avx2 () at ../sysdeps/x86_64/multiarch/strcpy-avx2.S:127
#1  0x00005ad51c60a37f in func2 (msg="test") at core_test1.cpp:8
#2  0x00005ad51c60a3c0 in func1 (msg="test") at core_test1.cpp:11
#3  0x00005ad51c60a42f in main () at core_test1.cpp:14
```

## 其它方式查看core dump

### 安裝coredump

```
sudo apt install systemd-coredump
```

### 執行並core dump

進入coredumpctl，使用q可離開

```
$ g++ -o demo03 core_test.cpp -g
$ ./demo03
程式記憶體區段錯誤 (核心已傾印
$ coredumpctl
TIME                          PID  UID  GID SIG     COREFILE EXE                       >
Thu 2024-12-05 15:17:05 CST 25260 1000 1000 SIGSEGV present  /home/cici/test/app/demo03>
Thu 2024-12-05 15:26:49 CST 25335 1000 1000 SIGSEGV present  /home/cici/test/app/demo03>
$ coredumpctl debug demo03
```

### bt

以下用黃色表示會要使用者輸入的部分與發生錯誤的行數。

bt可以列出呼叫函式的堆疊(call stack)，由下往上呼叫，最下面是main()呼叫func1()，func1()再呼叫func2()，最上層的是func2()

以下，使用q可離開
<pre>
<span class="markline">Enable debuginfod for this session? (y or [n]) y</span>
Debuginfod has been enabled.
To make this setting permanent, add 'set debuginfod enabled on' to .gdbinit.
[Thread debugging using libthread_db enabled]
--Type < RET > for more, q to quit, c to continue without paging--<span class="markline">c</span>
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Core was generated by `./demo03'.
Program terminated with signal SIGSEGV, Segmentation fault.
#0  0x00005893c17b7321 in func2 (msg=...) at core_test.cpp:8
<span class="markline">8	  *ptr = 97;  // 97代表a</span>
(gdb) <span class="markline">bt</span>
#0  0x00005893c17b7321 in func2 (msg="test") at core_test.cpp:8
#1  0x00005893c17b7365 in func1 (msg="test") at core_test.cpp:11
#2  0x00005893c17b73d4 in main () at core_test.cpp:14
</pre>

若發生錯誤的是在c++的函式庫中，怎麼查找錯誤？

將程式碼
```
*ptr = 97;
```
修改成c++函式strcpy
```
strcpy(ptr, msg.c_str());
```

修改程式碼
{% highlight c++ linenos %}
#include <cstring>
#include <iostream>
using namespace std;
void func2(const string msg) {
  // 宣告nullptr的ptr指標
  char * ptr = nullptr;
  // 試圖給nullptr設值
  strcpy(ptr, msg.c_str());
}
void func1(const string msg) {
  func2(msg);
}
int main() {
  func1("test");
  return 0;
}
{% endhighlight %}

重新編譯
```
$ g++ -o demo03 core_test.cpp -g
$ ./demo03
程式記憶體區段錯誤 (核心已傾印
$ coredumpctl
$ coredumpctl debug demo03
```

使用bt，就可以查找出來發生錯誤的函式，main()呼叫func1，func1呼叫func2，func2呼叫strcpy
<pre>
#0  __strcpy_avx2 () at ../sysdeps/x86_64/multiarch/strcpy-avx2.S:127

warning: 127	../sysdeps/x86_64/multiarch/strcpy-avx2.S: 沒有此一檔案或目錄
(gdb) <span class="markline">bt</span>
#0  __strcpy_avx2 () at ../sysdeps/x86_64/multiarch/strcpy-avx2.S:127
#1  0x000055a25facd37f in func2 (msg="test") at core_test.cpp:<span class="markline">8</span>
#2  0x000055a25facd3c0 in func1 (msg="test") at core_test.cpp:<span class="markline">11</span>
#3  0x000055a25facd42f in main () at core_test.cpp:<span class="markline">14</span>
</pre>

行數請比對如下
<pre>
  1 #include <cstring>
  2 #include <iostream>
  3 using namespace std;
  4 void func2(const string msg) {
  5   // 宣告nullptr的ptr指標
  6   char * ptr = nullptr;
  7   // 試圖給nullptr設值
 <span class="markline"> 8   strcpy(ptr, msg.c_str());</span>
  9 }
 10 void func1(const string msg) {
 <span class="markline">11   func2(msg);</span>
 12 }
 13 int main() {
 <span class="markline">14   func1("test");</span>
 15   return 0;
 16 }
</pre>