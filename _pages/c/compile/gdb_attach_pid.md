---
title: gdb attach pid
date: 2024-12-06
keywords: c++, gdb, gdb attach pid
---

Debug正在運行的程式


## 建立測試檔案

以下檔案每一秒印出1個數字
{% highlight c++ linenos %}
#include <unistd.h>
#include <cstring>
#include <iostream>
using namespace std;
void func2(const string msg) {
  for (int i = 0; i < 1000000; i++) {
    sleep(1);
    cout << "i = " << i << endl;
  }
}
void func1(const string msg) {
  func2(msg);
}
int main() {
  func1("test");
  return 0;
}
{% endhighlight %}

執行以下程式
```
$ g++ -o demo03 core_test.cpp -g
$ ./demo03
```

## ssh

遠端登入另一個終端機
```
ssh cici@192.168.235.128
```

## ps -ef 

列出跟demo03有關的process id

黃色標註就是demo03的pid
<pre>
cici@cici-vm:~/test/app$ ps -ef |grep demo03
cici       <span class="markline">27615</span>   26646  0 10:01 pts/7    00:00:00 ./demo03
cici       27618   26755  0 10:02 pts/5    00:00:00 grep --color=auto demo03
</pre>

## sudo gdb -p

以user的身份執行以下指令

```
sudo gdb /home/cici/test/app/demo03 -p 27615
```

會發現正在運行的程式停住了，輸入q離開gdb，正在運行的程式繼續運作。

## bt

列出呼叫函式的堆疊(call stack)

由下往上呼叫，最下面是main()呼叫func1()，func1()再呼叫func2(), func2()呼叫__sleep()

<pre>
(gdb) bt
#0  0x0000702da76eca7a in __GI___clock_nanosleep (clock_id=clock_id@entry=0, 
    flags=flags@entry=0, req=req@entry=0x7ffdb28b2040, 
    rem=rem@entry=0x7ffdb28b2040)
    at ../sysdeps/unix/sysv/linux/clock_nanosleep.c:78
#1  0x0000702da76f9a27 in __GI___nanosleep (req=req@entry=0x7ffdb28b2040, 
    rem=rem@entry=0x7ffdb28b2040) at ../sysdeps/unix/sysv/linux/nanosleep.c:25
 <span class="markline">#2  0x0000702da770ec63 in __sleep (seconds=0) at ../sysdeps/posix/sleep.c:55
#3  0x0000595ced2493ac in func2 (msg="test") at core_test.cpp:7
#4  0x0000595ced249437 in func1 (msg="test") at core_test.cpp:12
#5  0x0000595ced2494de in main () at core_test.cpp:15</span>
</pre>

## set var

目前i = 282
```
i = 268
i = 269
i = 270
i = 271
i = 272
i = 273
i = 274
i = 275
i = 276
i = 277
i = 278
i = 279
i = 280
i = 281
i = 282
```

將執行時的變數修改成500，以下黃色則為要輸入的
<pre>
(gdb) <span class="markline">n</span>
80	in ../sysdeps/unix/sysv/linux/clock_nanosleep.c
(gdb) <span class="markline">n</span>
83	in ../sysdeps/unix/sysv/linux/clock_nanosleep.c
(gdb) <span class="markline">n</span>
__GI___nanosleep (req=req@entry=0x7ffdb28b2040, rem=rem@entry=0x7ffdb28b2040)
    at ../sysdeps/unix/sysv/linux/nanosleep.c:26
warning: 26	../sysdeps/unix/sysv/linux/nanosleep.c: 沒有此一檔案或目錄
(gdb) <span class="markline">n</span>
__sleep (seconds=0) at ../sysdeps/posix/sleep.c:62
warning: 62	../sysdeps/posix/sleep.c: 沒有此一檔案或目錄
(gdb) <span class="markline">n</span>
64	in ../sysdeps/posix/sleep.c
(gdb) <span class="markline">n</span>
func2 (msg="test") at core_test.cpp:8
8	    cout << "i = " << i << endl;
(gdb) <span class="markline">set var i=500</span>
(gdb) <span class="markline">n</span>
6	  for (int i = 0; i < 1000000; i++) {
(gdb)
</pre> 

另一個視窗已經變成
<pre>
i = 268
i = 269
i = 270
i = 271
i = 272
i = 273
i = 274
i = 275
i = 276
i = 277
i = 278
i = 279
i = 280
i = 281
i = 282
<span class="markline">i = 500</span>
</pre>
