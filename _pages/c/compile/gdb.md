---
title: gdb
date: 2024-12-04
keywords: c++, gdb
---

## 何謂gdb

可以在linux系統下，debug c++程式碼，也可以debug正在運作的c++程式碼。

## 安裝gdb

```
sudo apt install gdb
```

## gdb除錯

### 欲除錯的程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void printParam(const char *param1, const char *param2, const char *param3) {
	cout << "姓名1 = " << param1 << endl;
	cout << "姓名2 = " << param2 << endl;
	cout << "姓名3 = " << param3 << endl;
}
int main(int argc, char *argv[], char *envp[]) {
  cout << "參數有" << argc << "個" << endl;
  for (int i = 0; i < argc; i++) {
    cout << "參數" << i << " = " << argv[i] << endl;
  }
  if (argc != 4) {
    cout << "使用方法:./demo 參數1 參數2 參數3 \n"; 
    return -1;
  }
  printParam(argv[1], argv[2], argv[3]);
  cout << "程式即將結束" << endl;
  return 0;
}
{% endhighlight %}

### 編譯語法

編譯執行檔時要加上-g，產生原始檔而不是只有機器語言指令。
```
g++ -o demo02 print_param.cpp -g
```

### 執行gdb
```
gdb demo02
```

### 離開gdb

按q

```
q
```

### 設置參數

語法
```
set args 參數1 參數2 參數3 參數N 
```
```
(gdb) set args cici bill mary
```

### vi看行數

法1:移到想斷點的行數，按 ctl + g

法2: 

按esc，輸入
```
:set number
```

關閉number 
```
:set nonumber
```
### 設置斷點break point

```
b 行數
```

```
(gdb) b 11
```

我在以下這行加上斷點
```
cout << "參數" << i << " = " << argv[i] << endl;
```

### 運行程式

程式運行到斷點的位置會停下來，若沒有斷點，程式會直接運行到結束。
```
r
```

### 執行

執行當前程式碼語句，不會進入函式內部。
```
n
```

```
Breakpoint 1, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:11
11	    cout << "參數" << i << " = " << argv[i] << endl;
(gdb) n
參數0 = /home/cici/test/app/demo02
```

### 執行下一行

再按一次n，就會執行下一行，不會進入函式內部，注意！最左側11與10是目前執行的行數

```
Breakpoint 1, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:11
11	    cout << "參數" << i << " = " << argv[i] << endl;
(gdb) n
參數0 = /home/cici/test/app/demo02
10	  for (int i = 0; i < argc; i++) {
(gdb) n

Breakpoint 1, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:11
11	    cout << "參數" << i << " = " << argv[i] << endl;
(gdb) 
```

### 印出變數

```
p 變數
```

```
Breakpoint 1, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:11
11	    cout << "參數" << i << " = " << argv[i] << endl;
(gdb) p i
$1 = 1
```

### 設置變數(法1)

離開gdb，重新執行gdb，設置參數```set args cici bill mary```，並把斷點設成11行，按下r，移到斷點

```
p 變數=值
```

因參數有4個(demo02,cici,bill,mary)


假設把i值直接設成3，再執行一次n(移到第10行)，再執行一次n(移到13行)，就會跳離迴圈，中間直接略過i=1,i=2

```
(gdb) p i=3
```

```
Breakpoint 1, main (argc=1, argv=0x7fffffffe348, envp=0x7fffffffe358) at print_param.cpp:11
11	    cout << "參數" << i << " = " << argv[i] << endl;
(gdb) p i
$1 = 0
(gdb) p i=3
$2 = 3
(gdb) n
參數3 = PWD=/home/cici/test/app
10	  for (int i = 0; i < argc; i++) {
(gdb) n
13	  if (argc != 4) {
```

### 設置變數(法2)

```
set var i=3
```
與
```
p i=3
```
是相同效果

### 直接運行到下一個斷點

輸入c會直接跳到下個斷點，若沒有下一個斷點，程式會直接運行到結束。
```
c
```

以下重新gdb，並設置斷點b 13與b 17，r是運行(會運行到第1個斷點13行)，再按c運行到下一個斷點(17行)。

```
cici@cici-vm:~/test/app$ g++ -o demo02 print_param.cpp -g
cici@cici-vm:~/test/app$ gdb demo02
(gdb) set args cici bill mary
(gdb) b 13
Breakpoint 1 at 0x1350: file print_param.cpp, line 13.
(gdb) b 17
Breakpoint 2 at 0x1376: file print_param.cpp, line 17.
(gdb) r

參數有4個
參數0 = /home/cici/test/app/demo02
參數1 = cici
參數2 = bill
參數3 = mary

Breakpoint 1, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:13
13	  if (argc != 4) {
(gdb) c
Continuing.

Breakpoint 2, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:17
17	  printParam(argv[1], argv[2], argv[3]);
(gdb) 
```

### 進入函式

17行剛好是呼叫函式printParam()的程式碼，按s，進入函式

```
s
```

```
Breakpoint 1, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:13
13	  if (argc != 4) {
(gdb) c
Continuing.

Breakpoint 2, main (argc=4, argv=0x7fffffffe328, envp=0x7fffffffe350) at print_param.cpp:17
17	  printParam(argv[1], argv[2], argv[3]);
(gdb) s
printParam (param1=0x7fffffffe5d5 "cici", param2=0x7fffffffe5da "bill", 
    param3=0x7fffffffe5df "mary") at print_param.cpp:4
4		cout << "姓名1 = " << param1 << endl;
(gdb) p param1
$1 = 0x7fffffffe5d5 "cici"
(gdb) p param2
$2 = 0x7fffffffe5da "bill"
(gdb) c
Continuing.
姓名1 = cici
姓名2 = bill
姓名3 = mary
程式即將結束
[Inferior 1 (process 22467) exited normally]
(gdb) 
```
