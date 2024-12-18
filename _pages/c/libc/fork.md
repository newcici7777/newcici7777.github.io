---
title: fork
date: 2024-12-17
keywords: linux, fork
---

## fork

現在的程序中，使用fork()創建子程序，子程序的程式碼執行的位置由fork()開始的程式碼，父程序fork()函式的傳回值是子程序的pid，子程序fork()傳回值為0。

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
-e：顯示所有進程，等同於 -A 選項。

-f：顯示完整的格式，包括父進程和其他資訊。

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

## 取得父程序的id

```
pid_t getpid(void);  // 取得目前程序pid
pid_t getppid(void);  //取得父程序pid
```