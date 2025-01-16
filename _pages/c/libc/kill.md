---
title: kill
date: 2024-12-13
keywords: c++, kill()
---

Prerequisites:
- [在linux編譯c++][1]
- [signal][2]
- [fork][3]

linux提供kill指令向程序(process)發送訊號。

語法
```
int kill(pid_t pid, int sig);
```
kill函式會將參數sig訊號傳給相關pid的程序(process)。

- pid > 0 傳給訊號給特定的pid程序。
- pid = 0 傳給訊號父pid相關的程序，包含父pid程序，此父pid程序的所有子程序都會收到sig訊號
- pid = -1 廣播所有process程序

以下程式碼<span class="mark">不能用kill -9 父pid</span>

使用以下二種方法測試
```
kill -15 父pid
或
ctrl+c
```

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <signal.h>
using namespace std;
void FatherExit(int sig) {
  // 怱略二次收到相同訊號
  // 使用kill(0, 訊號)會發給子程序也會再次發給發送訊號的父程序自己
  signal(SIGINT, SIG_IGN);  // 怱略ctrl+c
  signal(SIGTERM, SIG_IGN);  // 怱略kill -15
  cout << "父程序離開， signal = " << sig << endl;
  kill(0, SIGTERM);  // 向全部的子程序發送15訊號，終止子程序
  exit(0);  // 父程序離開
}
void ChildExit(int sig) {
  // 防止再次收到相同訊號
  signal(SIGINT, SIG_IGN);  // 怱略ctrl+c
  signal(SIGTERM, SIG_IGN);  // 怱略kill -15
  cout << "子程序 pid = " << getpid() << " 離開， signal = " << sig << endl;
  exit(0);
}
int main() {
  // 略過全部訊號
  for (int i = 0; i <= 64; i++) signal(i, SIG_IGN);
  // 父程序要補捉的訊號 ctrl + c
  signal(SIGTERM, FatherExit);
  // 父程序要補捉kill -15
  signal(SIGINT, FatherExit);
  while (true) {
    if (fork() > 0) {  // 建立子程序
      // 父程序
      sleep(5);  // 睡5秒
      continue;  // 回到迴圈判斷式中重頭執行
    } else {
      // 子程序
      // 子程序要補捉的訊號 ctrl + c
      signal(SIGTERM, ChildExit);
      // 子程序要補捉kill -15
      signal(SIGINT, ChildExit);
      while (true) {
        // 睡3秒
        sleep(3);
        cout << "子程序 pid = " << getpid() << " 正在執行中..." << endl;
      }
    }
  }
}
{% endhighlight %}

測試指令
```
$ps -ef | grep kill_test
cici      156524  139698  0 15:32 pts/3    00:00:00 ./kill_test
cici      156525  156524  0 15:32 pts/3    00:00:00 ./kill_test
cici      156526  156524  0 15:32 pts/3    00:00:00 ./kill_test
cici      156527  156524  0 15:32 pts/3    00:00:00 ./kill_test
cici      156533  156524  0 15:32 pts/3    00:00:00 ./kill_test
cici      156535  145543  0 15:32 pts/6    00:00:00 grep --color=auto kill_test
$ kill -15 156524
```

執行結果

父程序收到15訊號後離開，下面的所有子程序全跟著離開。

```
子程序 pid = 156527 正在執行中...
子程序 pid = 156540 正在執行中...
子程序 pid = 156536 正在執行中...
子程序 pid = 156526 正在執行中...
子程序 pid = 156539 正在執行中...
子程序 pid = 156533 正在執行中...
子程序 pid = 156538 正在執行中...
子程序 pid = 156525 正在執行中...
父程序離開， signal = 15
子程序 pid = 156527 離開， signal = 15
子程序 pid = 156540 離開， signal = 15
子程序 pid = 156541 離開， signal = 15
子程序 pid = 156539 離開， signal = 15
子程序 pid = 156533 離開， signal = 15
子程序 pid = 156526 離開， signal = 15
子程序 pid = 156537 離開， signal = 15
子程序 pid = 156536 離開， signal = 15
子程序 pid = 156538 離開， signal = 15
```

以下是使用ctrl+c，測試結果，父程序收到2訊號後離開，下面的所有子程序全跟著離開。

```
子程序 pid = 156636 正在執行中...
子程序 pid = 156636 正在執行中...
子程序 pid = 156637 正在執行中...
子程序 pid = 156636 正在執行中...
子程序 pid = 156637 正在執行中...
子程序 pid = 156636 正在執行中...
子程序 pid = 156638 正在執行中...
子程序 pid = 156637 正在執行中...
子程序 pid = 156636 正在執行中...
^C父程序離開， signal = 2
子程序 pid = 156641 離開， signal = 2
子程序 pid = 156638 離開， signal = 15
子程序 pid = 156637 離開， signal = 15
```

[1]: {% link _pages/c/compile/makefile.md %}
[2]: {% link _pages/c/libc/signal.md %}
[3]: {% link _pages/c/libc/fork.md %}