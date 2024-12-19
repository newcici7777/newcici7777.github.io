---
title: fork
date: 2024-12-17
keywords: linux, fork
---

## fork

現在的進程中，使用fork()創建子進程，子進程的程式碼執行的位置由fork()開始的程式碼，父進程fork()函式的傳回值是子進程的pid，子進程fork()傳回值為0。

fork_test.cpp
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  cout << "父進程開始" << endl;
  // 建立子進程，子進程開始的程式碼
  pid_t pid = fork();
  cout << "fork() return pid = " << pid << endl;
  // 停止30秒，方便以ps -ef | grep 查看父進程與子進程
  sleep(30);
  cout << "結束" << endl;
}
{% endhighlight %}

```
父進程開始
fork() return pid = 145656
fork() return pid = 0
結束
結束
```

由執行結果可以知道，`父進程開始`的字只執行一次。

執行上面的程式，打開另一個終端機視窗
```
ps -ef | grep fork
```
-e：顯示所有進程，等同於 -A 選項。

-f：顯示完整的格式，包括父進程和其他資訊。

可以發現子進程pid是145773，父進程是145772，有二個進程都跑起來了。
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

由以上結果可以知，145772下面有一個子進程145773

## 透過fork傳回值執行不同程式碼

父進程fork()函式的傳回值是子進程的pid，子進程fork()傳回值為0，根據傳回值判斷是父進程或子進程。

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

fork()函式，會把變數複製一份給子進程，雖然子進程與父進程的變數位址是相同，但實際上是存放不同位址。
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
    // 父進程
    cout << "父get fork pid = " << pid << endl;
    cout << "id = " << id << ", address = " << &id << endl;
    cout << "name = " << name << ", address = " << &name << endl;
  } else {
    // 子進程
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

以下程式碼先讓子進程先執行(子進程有修改變數的值)，5秒後，再執行父進程
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
    // 先讓父進程先睡5秒，讓子進程先執行
    sleep(5);
    // 父進程
    cout << "父get fork pid = " << pid << endl;
    cout << "id = " << id << ", address = " << &id << endl;
    cout << "name = " << name << ", address = " << &name << endl;
  } else {
    // 修改變數值
    id = 777;
    name = "Bill";
    // 子進程
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

由執行結果可以看出，即便子進程修改了變數值，但父進程仍顯示原來的值(888與Mary)。

## 子進程在後台執行

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

如果 fork() 的返回值大於 0，表示這是 父進程，執行 return 0 直接結束父進程。

如果 fork() 的返回值等於 0，表示這是 子進程，繼續執行後面的程式碼。

父進程結束後，只有子進程執行程式的剩餘部分。

子進程會進入無限迴圈，並每秒輸出 第X秒。

fork_test.cpp
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  // 父進程退出，只剩下fork的子進程在執行
  if (fork() > 0) return 0;
  // 無限迴圈
  for (int i = 0; ; i++) {
    cout << "第" << i << "秒" << endl;
    sleep(1);
  }
}
{% endhighlight %}

### 孤兒進程orphan process

父進程結束，子進程沒有父親，子進程變成孤兒，由init系統進程(1號進程)接管

使用ps查看父進程的pid已經變成1號進程

```
$ ps -ef | grep fork_test
cici      154291       1  0 09:46 pts/3    00:00:00 ./fork_test
```

### 刪除後台進程

開另一個終端機，輸入`killall -9 進程名`或`kill 進程pid`

## 0號進程與1號進程與2號進程


### 0號進程

名稱：Idle 進程（也稱為 swapper 或 scheduler）

特性：

內核中的第一個進程，在系統啟動階段由內核創建。

它的主要功能是用來初始化系統並為其他進程提供基礎服務。

並不執行任何用戶程式，通常只執行在內核空閒時需要完成的工作。

作用：

創建 1 號進程（init 或現代系統中的 systemd）。

作為空閒進程，它在沒有其他進程需要執行時，負責將 CPU 設置為低功耗狀態（通常進入 hlt 指令）。

用戶看不見：

雖然它是 0 號進程，但不會出現在常規的 ps 或 top 顯示中，因為它是完全運行在內核態的。

### 1號進程

名稱：init 進程（或 systemd）

特性：

由 0 號進程創建，為操作系統中的第一個用戶態進程。

PID（進程 ID）為 1，因此被稱為 1 號進程。

作用：

負責系統初始化，如加載其他守護進程、掛載檔案系統、設置網絡等。

它是所有用戶態進程的祖先，因為所有其他進程都是由它派生（直接或間接）出來的。

現代系統中，systemd 通常替代了傳統的 init 進程，擁有更多功能，如並行加載服務。

出現在系統中：

可以通過 ps -e 或 top 查看到它，進程名稱通常是 init 或 systemd。

特殊作用：

如果父進程終止，孤兒進程會由 1 號進程接管，確保它們被妥善清理。

### 2號進程

名稱：kthreadd（內核線程管理器）

特性：

它是 0 號進程創建的另一個進程。

它的主要功能是內核線程的管理器，負責處理所有內核線程的生成。

作用：

負責啟動和管理內核態的線程，這些線程執行例如磁碟 I/O、網絡處理等系統級操作。

它創建的內核線程通常以 k 開頭（例如 kworker、ksoftirqd 等）。

特點：

像 0 號進程一樣，運行在內核態，但它的子線程會執行具體的內核任務。

用戶可以在系統中通過工具（如 ps -e）看到它。


### 進程之間的關係
啟動順序：

系統啟動時，最初由內核創建 0 號進程。

0 號進程創建 1 號進程（用戶態的第一個進程）。

0 號進程也會創建 2 號進程，來管理內核態的線程。

拓展：

1 號進程負責啟動所有其他用戶態進程（例如登錄管理器、系統服務等）。

2 號進程負責啟動和管理內核態進程。

### 查看0號進程樹
語法
```
pstree -p 0
```

## zombie process

父進程沒有處理子進程的退出(如:資源釋放)，造成僵屍進程，子進程的pid就會一直存在，系統的pid數量是有限的，若存在太多子進程pid，就不會再產生新的進程。

使用以下程式碼會產生僵屍進程

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
using namespace std;
int main() {
  // 退出子進程
  if (fork() == 0) return 0;
  // 無限迴圈
  for (int i = 0; ; i++) {
    cout << "父程序運行第" << i << "秒" << endl;
    sleep(1);
  }
}
{% endhighlight %}

執行時，再用ps查詢進程，可以發現明明子程序154631已經退出，但仍占著pid，後面跟著`< defunct >`，這就是僵屍進程

```
$ ps -ef | grep fork_test
cici      154630  139698  0 10:19 pts/3    00:00:00 ./fork_test
cici      154631  154630  0 10:19 pts/3    00:00:00 [fork_test] <defunct>
```

避免僵屍進程的方法

### 怱略SIGCHLD

子進程退出前，kernel會向父進程發出SIGCHLD信號，把它怱略，kernel收到會釋放子進程資源。

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
  // 呼叫fork()之前，怱略子進程退出的信號
  signal(SIGCHLD, SIG_IGN);
  // 退出子進程
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
- pid_t:傳回子進程pid
- stat_loc指標:用於存放子程序退出的結果

正常退出為以下的狀況，不管x值是什麼，都是正常退出。

- return x;
- exit(x);
- `_exit(x)`或`_Exit(x)`

異常退出

- 收到信號`kill 程序pid`
- abort()函式終止

#### 正常退出程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>
using namespace std;
int main() {
  if (fork() > 0) {
    // 父進程
    int status;  // 用於儲存子進程退出的結果
    pid_t pid = wait(&status);  // 等待子進程退出
    cout << "已終止的子進程pid是:" << pid << endl;
    // 把退出結果傳給WIFEXITED()前置指令，若回傳true，代表正常退出來
    if (WIFEXITED(status)) {
      cout << "正常退出，狀態 : " << WEXITSTATUS(status) << endl;
    } else {
      cout << "異常退出，狀態 : " << WTERMSIG(status) << endl;
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
已終止的子進程pid是:154866
正常退出，狀態 : 5
```

#### 使用kill不正常退出1

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
已終止的子進程pid是:154939
異常退出，狀態 : 15
```

#### 使用kill不正常退出2

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
已終止的子進程pid是:155001
異常退出，狀態 : 9
```

#### 操作nullptr不正常退出

在子進程的部分，增加操作nullptr的程式碼
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
    // 父進程
    int status;  // 用於儲存子進程退出的結果
    pid_t pid = wait(&status);  // 等待子進程退出
    cout << "已終止的子進程pid是:" << pid << endl;
    // 把退出結果傳給WIFEXITED()前置指令，若回傳true，代表正常退出來
    if (WIFEXITED(status)) {
      cout << "正常退出，狀態 : " << WEXITSTATUS(status) << endl;
    } else {
      cout << "異常退出，狀態 : " << WTERMSIG(status) << endl;
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
已終止的子進程pid是:155059
異常退出，狀態 : 11
```

## 取得父進程的id

```
pid_t getpid(void);  // 取得目前進程pid
pid_t getppid(void);  //取得父進程pid
```