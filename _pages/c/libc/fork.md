---
title: fork 複製程序
date: 2024-12-17
keywords: linux, fork
---

## fork

現在的程序中，使用fork()複製出子程序，子程序的程式碼執行的位置由fork()開始的程式碼，父程序fork()函式的傳回值是子程序的pid，子程序fork()傳回值為0。

fork_test.cpp
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  cout << "父程序開始" << endl;
  // 建立子程序，子程序開始的程式碼
  pid_t pid = fork();
  cout << "fork() return pid = " << pid << endl;
  // 停止30秒，方便以ps -ef | grep 查看父程序與子程序
  sleep(30);
  cout << "結束" << endl;
}
{% endhighlight %}

```
父程序開始
fork() return pid = 145656
fork() return pid = 0
結束
結束
```

由執行結果可以知道，`父程序開始`的字只執行一次。

執行上面的程式，打開另一個終端機視窗
```
ps -ef | grep fork
```
-e：顯示所有程序，等同於 -A 選項。

-f：顯示完整的格式，包括父程序和其他資訊。

可以發現子程序pid是145773，父程序是145772，有二個程序都跑起來了。
```
cici      145772  139698  0 14:43 pts/3    00:00:00 ./fork_test
cici      145773  145772  0 14:43 pts/3    00:00:00 ./fork_test
```

使用pstree查看子父關係

語法
```
pstree -p 父pid
```

```
$ pstree -p 145772
fork_test(145772)───fork_test(145773)
```

由以上結果可以知，145772下面有一個子程序145773

## 透過fork傳回值執行不同程式碼

父程序fork()函式的傳回值是子程序的pid，子程序fork()傳回值為0，根據傳回值判斷是父程序或子程序。

父get fork pid = 146095

子get fork pid = 0

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  pid_t pid = fork();
  if (pid > 0) {
    cout << "父get fork pid = " << pid << endl;
  } else {
    cout << "子get fork pid = " << pid << endl;
  }
  sleep(20);
  cout << "結束" << endl;
}
{% endhighlight %}
```
父get fork pid = 146095
子get fork pid = 0
結束
結束
```

## fork()與變數

fork()函式，會把變數複製一份給子程序，雖然子程序與父程序的變數位址是相同，但實際上是存放不同位址。
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  // 學號
  int id = 888;
  // 姓名
  string name = "May";
  // fork傳回值pid
  pid_t pid = fork();
  if (pid > 0) {
    // 父程序
    cout << "父get fork pid = " << pid << endl;
    cout << "id = " << id << ", address = " << &id << endl;
    cout << "name = " << name << ", address = " << &name << endl;
  } else {
    // 子程序
    cout << "子get fork pid = " << pid << endl;
    cout << "id = " << id << ", address = " << &id << endl;
    cout << "name = " << name << ", address = " << &name << endl;
  }
  sleep(20);
  cout << "結束" << endl;
}
{% endhighlight %}
```
父get fork pid = 146276
id = 888, address = 0x7ffef9691170
name = May, address = 0x7ffef9691180
子get fork pid = 0
id = 888, address = 0x7ffef9691170
name = May, address = 0x7ffef9691180
結束
結束
```

證明不是讀取相同記憶體位址

以下程式碼先讓子程序先執行(子程序有修改變數的值)，5秒後，再執行父程序
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  // 學號
  int id = 888;
  // 姓名
  string name = "May";
  // fork傳回值pid
  pid_t pid = fork();
  if (pid > 0) {
    // 先讓父程序先睡5秒，讓子程序先執行
    sleep(5);
    // 父程序
    cout << "父get fork pid = " << pid << endl;
    cout << "id = " << id << ", address = " << &id << endl;
    cout << "name = " << name << ", address = " << &name << endl;
  } else {
    // 修改變數值
    id = 777;
    name = "Bill";
    // 子程序
    cout << "子get fork pid = " << pid << endl;
    cout << "id = " << id << ", address = " << &id << endl;
    cout << "name = " << name << ", address = " << &name << endl;
  }
  sleep(20);
  cout << "結束" << endl;
}
{% endhighlight %}
```
子get fork pid = 0
id = 777, address = 0x7ffeb7027c90
name = Bill, address = 0x7ffeb7027ca0
父get fork pid = 146339
id = 888, address = 0x7ffeb7027c90
name = May, address = 0x7ffeb7027ca0
結束
結束
```

由執行結果可以看出，即便子程序修改了變數值，但父程序仍顯示原來的值(888與Mary)。

## 子程序在背景執行

子程序在背景執行時，不會被ctrl+c強制終止。

### 法1
語法
```
./執行檔名 &
```

### 法2

在程式碼第一行加上
```
if (fork() > 0) return 0;
```

編譯並執行，執行後面不用加&
```
./執行檔名 
```

如果 fork() 的返回值大於 0，表示這是 父程序，執行 return 0 直接結束父程序。

如果 fork() 的返回值等於 0，表示這是 子程序，繼續執行後面的程式碼。

父程序結束後，只有子程序執行程式的剩餘部分。

子程序會進入無限迴圈，並每秒輸出 第X秒。

fork_test.cpp
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  // 父程序離開，只剩下fork的子程序在執行
  if (fork() > 0) return 0;
  // 無限迴圈
  for (int i = 0; ; i++) {
    cout << "第" << i << "秒" << endl;
    sleep(1);
  }
}
{% endhighlight %}

### 孤兒程序orphan process

父程序結束，子程序沒有父親，子程序變成孤兒，由init系統程序(1號程序)接管

使用ps查看父程序的pid已經變成1號程序

```
$ ps -ef | grep fork_test
cici      154291       1  0 09:46 pts/3    00:00:00 ./fork_test
```

### 刪除背景程序

開另一個終端機，輸入`killall -9 程序名`或`kill 程序pid`

## PID 0 1 2 號


### PID 0 號

名稱：Idle 程序（也稱為 swapper 或 scheduler）

特性：

核心中的第一個程序，在系統啟動階段由核心創建。

### PID 1 號

名稱：systemd

由 0 號程序創建

作用：

負責系統初始化，如加載其他守護程序、掛載檔案系統、設置網路等。

### PID 2 號

名稱：kthreadd（核心多工管理器）

特性：

負責執行例如磁碟 I/O、網絡處理等系統級操作。

PID（程序 ID）為 2，因此被稱為 2 號程序。

### 程序之間的關係

啟動順序：

系統啟動時，最初由核心創建 0 號程序。

0 號程序創建 1 號程序

0 號程序也會創建 2 號程序

### 查看0號程序樹

語法
```
pstree -p 0
```

## zombie process

父程序沒有處理子程序的離開(如:資源釋放)，造成僵屍程序，子程序的pid就會一直存在，系統的pid數量是有限的，若存在太多子程序pid，就不會再產生新的程序。

使用以下程式碼會產生僵屍程序

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
using namespace std;
int main() {
  // 離開子程序
  if (fork() == 0) return 0;
  // 無限迴圈
  for (int i = 0; ; i++) {
    cout << "父程序運行第" << i << "秒" << endl;
    sleep(1);
  }
}
{% endhighlight %}

執行時，再用ps查詢程序，可以發現明明子程序154631已經離開，但仍占著pid，後面跟著`< defunct >`，這就是僵屍程序

```
$ ps -ef | grep fork_test
cici      154630  139698  0 10:19 pts/3    00:00:00 ./fork_test
cici      154631  154630  0 10:19 pts/3    00:00:00 [fork_test] <defunct>
```

避免僵屍程序的方法

### 怱略SIGCHLD

子程序離開前，kernel會向父程序發出SIGCHLD訊號，把它怱略，kernel收到會釋放子程序資源。

語法
```
signal(SIGCHLD, SIG_IGN);
```
SIG_IGN代表怱略

程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
using namespace std;
int main() {
  // 呼叫fork()之前，怱略子程序離開的訊號
  signal(SIGCHLD, SIG_IGN);
  // 離開子程序
  if (fork() == 0) return 0;
  // 無限迴圈
  for (int i = 0; ; i++) {
    cout << "父程序運行第" << i << "秒" << endl;
    sleep(1);
  }
}
{% endhighlight %}

執行程序時，使用ps查看，發現pid只剩下父程序154674
```
$ ps -ef | grep fork_test
cici      154674  139698  0 10:22 pts/3    00:00:00 ./fork_test
```

### 使用wait
include
```
#include <sys/wait.h>
```
語法
```
pid_t wait(int *stat_loc);
```
- pid_t:傳回子程序pid
- stat_loc指標:用於存放子程序離開的結果

正常離開為以下的狀況，不管x值是什麼，都是正常離開。

- return x;
- exit(x);
- `_exit(x)`或`_Exit(x)`

異常離開

- 收到訊號`kill 程序pid`
- abort()函式終止

#### 正常離開程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>
using namespace std;
int main() {
  if (fork() > 0) {
    // 父程序
    int status;  // 用於儲存子程序離開的結果
    pid_t pid = wait(&status);  // 等待子程序離開
    cout << "已終止的子程序pid是:" << pid << endl;
    // 把離開結果傳給WIFEXITED()前置指令，若傳回true，代表正常離開來
    if (WIFEXITED(status)) {
      cout << "正常離開，狀態 : " << WEXITSTATUS(status) << endl;
    } else {
      cout << "異常離開，狀態 : " << WTERMSIG(status) << endl;
    }
  } else {
    // 子程序執行15秒 0 .. 14
    for (int i = 0; i < 15 ; i++) {
      cout << "第" << i << "秒" << endl;
      sleep(1);
    }
    exit(5);
  }
}
{% endhighlight %}
```
第0秒
第1秒
第2秒
第3秒
第4秒
第5秒
第6秒
第7秒
第8秒
第9秒
第10秒
第11秒
第12秒
第13秒
第14秒
已終止的子程序pid是:154866
正常離開，狀態 : 5
```

#### 使用kill不正常離開1

- 執行程式
- 打開另一個終端機視窗
	```
	$ ps -ef | grep fork_test
	cici      154938  139698  0 10:50 pts/3    00:00:00 ./fork_test
	cici      154939  154938  0 10:50 pts/3    00:00:00 ./fork_test
	cici      154942  145543  0 10:50 pts/6    00:00:00 grep --color=auto fork_test
	$ kill 154939
	```
- 回到原本執行程式的終端機視窗
```
第0秒
第1秒
第2秒
第3秒
第4秒
第5秒
第6秒
第7秒
第8秒
第9秒
第10秒
第11秒
第12秒
已終止的子程序pid是:154939
異常離開，狀態 : 15
```

#### 使用kill不正常離開2

- 執行程式
- 打開另一個終端機視窗
	```
	$ ps -ef | grep fork_test
	cici      155000  139698  0 10:55 pts/3    00:00:00 ./fork_test
	cici      155001  155000  0 10:55 pts/3    00:00:00 ./fork_test
	cici      155003  145543  0 10:55 pts/6    00:00:00 grep --color=auto fork_test
	$ kill -9 155001
	```
- 回到原本執行程式的終端機視窗
```
第0秒
第1秒
第2秒
第3秒
第4秒
第5秒
第6秒
第7秒
第8秒
第9秒
第10秒
第11秒
第12秒
已終止的子程序pid是:155001
異常離開，狀態 : 9
```

#### 操作nullptr不正常離開

在子程序的部分，增加操作nullptr的程式碼
```
    int* ptr = nullptr;
    *ptr = 10;
```

|訊號名|訊號值|發出訊號原因|
|SIGSEGV|11|操作nullptr指標或超出陣列索引|

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>
using namespace std;
int main() {
  if (fork() > 0) {
    // 父程序
    int status;  // 用於儲存子程序離開的結果
    pid_t pid = wait(&status);  // 等待子程序離開
    cout << "已終止的子程序pid是:" << pid << endl;
    // 把離開結果傳給WIFEXITED()前置指令，若傳回true，代表正常離開來
    if (WIFEXITED(status)) {
      cout << "正常離開，狀態 : " << WEXITSTATUS(status) << endl;
    } else {
      cout << "異常離開，狀態 : " << WTERMSIG(status) << endl;
    }
  } else {
    // 子程序執行15秒 0 .. 14
    for (int i = 0; i < 15 ; i++) {
      cout << "第" << i << "秒" << endl;
      sleep(1);
    }
    int* ptr = nullptr;
    *ptr = 10;
    exit(5);
  }
}
{% endhighlight %}
```
第0秒
第1秒
第2秒
第3秒
第4秒
第5秒
第6秒
第7秒
第8秒
第9秒
第10秒
第11秒
第12秒
第13秒
第14秒
已終止的子程序pid是:155059
異常離開，狀態 : 11
```

### 補捉SIGCHLD

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>
using namespace std;
/**
 補捉SIGCHLD子程序離開訊號
 */
void func(int signal) {
  int status;  // 用於儲存子程序離開的結果
  pid_t pid = wait(&status);  // 等待子程序離開
  cout << "已終止的子程序pid是:" << pid << endl;
  // 把離開結果傳給WIFEXITED()前置指令，若傳回true，代表正常離開來
  if (WIFEXITED(status)) {
    cout << "正常離開，狀態 : " << WEXITSTATUS(status) << endl;
  } else {
    cout << "異常離開，狀態 : " << WTERMSIG(status) << endl;
  }
}
int main() {
  // 補捉子程序離開的訊號
  signal(SIGCHLD, func);
  if (fork() > 0) {
    // 父程序執行30秒 0 .. 29
    for (int i = 0; i < 30 ; i++) {
      cout << "父 : 第" << i << "秒" << endl;
      sleep(1);
    }
  } else {
    // 子程序執行15秒 0 .. 14
    for (int i = 0; i < 15 ; i++) {
      cout << "子 : 第" << i << "秒" << endl;
      sleep(1);
    }
    // 子程序執行15秒結束
    exit(5);
  }
}
{% endhighlight %}
```
父 : 第0秒
子 : 第0秒
父 : 第1秒
子 : 第1秒
父 : 第2秒
子 : 第2秒
父 : 第3秒
子 : 第3秒
父 : 第4秒
子 : 第4秒
父 : 第5秒
子 : 第5秒
父 : 第6秒
子 : 第6秒
父 : 第7秒
子 : 第7秒
父 : 第8秒
子 : 第8秒
父 : 第9秒
子 : 第9秒
父 : 第10秒
子 : 第10秒
父 : 第11秒
子 : 第11秒
父 : 第12秒
子 : 第12秒
父 : 第13秒
子 : 第13秒
父 : 第14秒
子 : 第14秒
父 : 第15秒
已終止的子程序pid是:155462
正常離開，狀態 : 5
父 : 第16秒
父 : 第17秒
父 : 第18秒
父 : 第19秒
父 : 第20秒
父 : 第21秒
父 : 第22秒
父 : 第23秒
父 : 第24秒
父 : 第25秒
父 : 第26秒
父 : 第27秒
父 : 第28秒
父 : 第29秒
```

## 取得父程序的id

```
pid_t getpid(void);  // 取得目前程序pid
pid_t getppid(void);  //取得父程序pid
```