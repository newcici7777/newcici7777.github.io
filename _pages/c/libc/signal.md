---
title: signal訊號
date: 2024-12-13
keywords: c++, signal
---
Prerequisites:

[在linux編譯c++][1]

kill或killall給程序(process)傳送訊號(signal)，也可以使用c函式庫(libc)的kill()函式給程序(process)傳送訊號(signal)。

## kill與killall
### kill 指令

用法：kill [選項] <PID>

功能：向特定的程序 ID (PID) 發送訊號（例如，結束程序）。

作用範圍：只能針對指定的程序 ID。

常見訊號：
- -9：強制終止程序（SIGKILL）。
- -15：預設訊號，請求程序正常離開（SIGTERM）。。

```
kill -9 1234  # 終止 PID 為 1234 的程序
```

### killall 指令

用法：killall [選項] <程序名稱>

功能：根據程序名稱來終止所有匹配的程序。

作用範圍：針對同名的所有程序。

```
killall -9 nginx  # 終止所有名為 "nginx" 的程序
```

### 常用的訊號:

|訊號名|訊號值|發出訊號原因|
|SIGINT|2|ctrl+c停止程序|
|SIGKILL|9|強制終止程序|
|SIGSEGV|11|操作nullptr指標或超出陣列索引|
|SIGALRM|14|alarm()函式發出訊號|
|SIGTERM|15|kill後不加任何-編號，請求程序正常離開|
|SIGCHLD|17|子程序結束|

## signal函式

使用signal()函式收訊號，收到訊號後指派函式去處理。

include
```
#include <signal.h>
```

```
sighandler_t signal(int signum, sighandler_t handler);
```

參數:
- signum 設置要接收的訊號
- handler 設置要處理訊號的函式

程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <signal.h>
#include <unistd.h>  // system() head file
using namespace std;
void func(int signum) {
  cout << "收到訊號 = " << signum << endl;
}
int main() {
  // 收到1的訊號，執行func函式
  signal(1, func);
  // 收到15的訊號，執行func函式
  signal(15, func);
  
  for (int i = 0; ; i++) {
    cout << "i = " << i << endl;
    sleep(1);
  }
  return 0;
}
{% endhighlight %}

編譯並執行
```
$ g++ -o signal_test signal_test.cpp
$ ./signal_test
```

打開另一個ssh連線到linux終端機，使用kill或killall傳送訊號1與訊號15
```
$ killall -1 signal_test
$ killall -15 signal_test
```

查看原本編譯執行的終端機，可以看出有收到1與15的訊號
<pre>
.
.
.
i = 68
<span class="markline">收到訊號 = 1</span>
i = 69
i = 70
i = 71
i = 72
i = 73
i = 74
i = 75
i = 76
i = 77
i = 78
i = 79
i = 80
i = 81
<span class="markline">收到訊號 = 15</span>
i = 82
i = 83
i = 84
i = 85
i = 86
.
.
.
</pre>

## ctrl+c終止程序

使用ctrl+c可以終止正在執行的程序，本頁範例皆為無限迴圈，需手動ctrl+c終止程序。

## SIG_IGN忽略訊號

```
sighandler_t signal(int signum, sighandler_t handler);
```
參數:
- handler 設置要處理訊號的函式

SIG_IGN放在handler的位置。

增加一行，怱略2的訊號
```
  // 收到2的訊號，不要執行func函式，SIG_IGN是忽略訊號
  signal(2, SIG_IGN);
```

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <signal.h>
#include <unistd.h>  // system() head file
using namespace std;
void func(int signum) {
  cout << "收到訊號 = " << signum << endl;
}
int main() {
  // 收到1的訊號，執行func函式
  signal(1, func);
  // 收到15的訊號，執行func函式
  signal(15, func);
  // 收到2的訊號，不要執行func函式，SIG_IGN是略過訊號
  signal(2, SIG_IGN);
  for (int i = 0; ; i++) {
    cout << "i = " << i << endl;
    sleep(1);
  }
  return 0;
}
{% endhighlight %}

編譯執行

使用killall指令發送訊號
```
killall -2 signal_test
```

## SIG_DFL恢復

```
sighandler_t signal(int signum, sighandler_t handler);
```
參數:
- handler 設置要處理訊號的函式

若原本是有透過signal()函式處理某個訊號，也可以使用SIG_DFL恢復原本收到訊息的狀況

SIG_DFL放在handler的位置。

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <signal.h>
#include <unistd.h>  // system() head file
using namespace std;
void func(int signum) {
  cout << "收到訊號 = " << signum << endl;
  // 恢復原本15訊號，程式不再接收15訊號
  signal(15, SIG_DFL);
}
int main() {
  // 收到15的訊號，執行func函式
  signal(15, func);
  for (int i = 0; ; i++) {
    cout << "i = " << i << endl;
    sleep(1);
  }
  return 0;
}
{% endhighlight %}

編譯執行

發送第一次15訊號
```
$ killall -15 signal_test
```

執行結果
```
i = 0
i = 1
i = 2
i = 3
i = 4
i = 5
i = 6
i = 7
i = 8
i = 9
i = 10
i = 11
i = 12
i = 13
i = 14
i = 15
i = 16
收到訊號 = 15
i = 17
```

發送第二次15訊號
```
$ killall -15 signal_test
```

執行結果
```
i = 13
i = 14
i = 15
i = 16
收到訊號 = 15
i = 17
i = 18
i = 19
i = 20
終止
```

由執行結果可以發現，第2次發送訊號是正常終止程序。

## 無法收到與略過的訊號

以下三種都是無法收到與略過的訊號
|訊號名|訊號值|發出訊號原因|
|SIGKILL|9|強制終止程序|
|SIGSEGV|11|操作nullptr指標或超出陣列索引|
|SIGSTOP|19|終止程序|

以下程式碼即便設定收9的訊號，但執行`killall -9 signal_test`，func()函式不會被執行，直接終止程序。
{% highlight c++ linenos %}
#include <iostream>
#include <signal.h>
#include <unistd.h>  // system() head file
using namespace std;
void func(int signum) {
  cout << "收到訊號 = " << signum << endl;
}
int main() {
  signal(9, func);
  for (int i = 0; ; i++) {
    cout << "i = " << i << endl;
    sleep(1);
  }
  return 0;
}
{% endhighlight %}

## alarm函式

alarm()是定時器，參數與秒數，秒數到了會自動發出14的訊號。

語法
```
alarm(5)
```

注意事項

- func()函式中，不能少alarm(5)，若func()函式中少了alram()，就只會啟動一次alarm，不能無限啟動。
- signal(14, func)設置在alarm()函式之後


程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <signal.h>
#include <unistd.h>  // system() head file
using namespace std;
void func(int signum) {
  cout << "收到訊號 = " << signum << endl;
  // 設置定時器，參數為5，代表5秒後啟動
  alarm(5);
}
int main() {
  // 設置定時器，參數為5，代表5秒後啟動
  alarm(5);
  // signal函式一定要放置alarm()後面
  // 設定收14的訊號
  signal(14, func);
  for (int i = 0; ; i++) {
    cout << i << "秒" << endl;
    sleep(1);
  }
  return 0;
}
{% endhighlight %}

執行結果每5秒發出14的訊號，由func()函式接收。

```
0秒
1秒
2秒
3秒
4秒
收到訊號 = 14
5秒
6秒
7秒
8秒
9秒
收到訊號 = 14
10秒
11秒
12秒
13秒
14秒
收到訊號 = 14
15秒
16秒
^C
```

## 0訊號

檢查程序是否正常運作。

對上一個例子使用`./alarm_test`

打開另一個終端機(使用ssh連線)，執行以下指令。

```
$ killall -0 signal_test
signal_test：找不到任何行程
$ killall -0 alarm_test
```
若程序沒執行會顯示`找不到任何行程`


[1]: {% link _pages/c/compile/makefile.md %}
