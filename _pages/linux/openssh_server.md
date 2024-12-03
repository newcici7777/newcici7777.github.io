---
title: 安裝openssh-server
date: 2024-12-02
keywords: Ubuntu, openssh-server
---

## openssh-server

以下環境

本機MAC

Ubuntu 24.04

VMware Fusion

以下為Ubuntu虛擬機終端機設定

- 更新package list
```
$ sudo apt update
```
- 安裝openssh-server
```
$ sudo apt install openssh-server
```
按下y

- 啟動ssh
```
$ sudo systemctl status ssh
```
離開按q
```
$ sudo systemctl enable --now ssh
```
- 本機連ssh
```
$ ssh localhost
```
按下y

- 重新啟動ssh
$ sudo systemctl restart ssh

## 安裝c++
```
$ sudo apt install gcc
$ sudo apt install g++
$ sudo apt install build-essential
$ gcc --version
$ g++ --version
```
## 安裝man page
```
$ sudo apt install manpages-dev manpages-posix-dev
$ man strcpy
```
按q退出
若跟user command有衝突的函式，請在函式名前輸入3
```
$ man 3 sleep
```
1是user command
3是Standard C library

## 編譯c與c++

### gcc與g++分別

gcc編譯c

g++編譯c++

### g++語法

```
g++ -o 執行檔名 原始檔案1.cpp 原始檔案2.cpp 原始檔案3.cpp 原始檔案N.cpp
```

常用選項:

-o 設定\"執行檔名\"

-std=c++11 支援c++11

-c 只編譯，不鏈結

### 產生執行檔

#### 建立檔案
```
$ vi hello.cpp
```
將以下的內容貼上
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  cout << "Hello world!\n";
}
{% endhighlight %}

#### 產生執行檔
```
$ g++ -o demo hello.cpp
```
#### 執行
```
$ ./demo
```
Hello world!

#### 沒有-o 執行檔名

沒有-o 執行檔名，會產生a.out的檔案
```
$ g++ hello.cpp
$ ls
```
a.out 

#### 使用2個cpp產生執行檔

$ vi public.h
{% highlight c++ linenos %}
#include <iostream>
void func();
{% endhighlight %}

$ vi public.cpp
{% highlight c++ linenos %}
#include "public.h"
using namespace std;
void func() {
  cout << "call func()" << endl;
}
{% endhighlight %}

$ vi hello.cpp
{% highlight c++ linenos %}
#include <iostream>
#include "public.h"
using namespace std;
int main() {
  cout << "Hello world!\n";
  func();
}
{% endhighlight %}

```
$ g++ -o demo hello.cpp public.cpp
$ ./demo
```

```
Hello world!
call func()
```

## 靜態庫

將cpp檔案整理成一個庫，產生執行檔可以指定庫名.a，就不用寫很多原始檔案1.cpp 原始檔案2.cpp 原始檔案3.cpp 原始檔案N.cpp

多個procee用到同一個靜態庫中的函式或類別，每個process拷貝一份，多個process就有多個拷貝。

靜態庫是二進制檔案

主程式在鏈結時會把靜態庫加入，產生執行檔。

若某個靜態庫更新了，所有使用它的程式都需要重新編譯。

### 語法
```
g++ -c -o lib庫名.a 原始檔案1.cpp 原始檔案2.cpp 原始檔案3.cpp 原始檔案N.cpp
```
#### -c

-c 只編譯，不鏈結

#### lib庫名.a

.a代表靜態庫

lib是固定

庫名可以自由變換

### 檔案放置路徑
```
cici@cici-vm:~/test$ tree
```
.
├── app
│   └── hello.cpp
└── tools
    ├── public.cpp
    └── public.h

3 directories, 3 files


將以下檔案依上方目錄結構放置。

### public.h
{% highlight c++ linenos %}
#include <iostream>
void func();
class Student {
 public:
  void print();
};
{% endhighlight %}

### public.cpp
{% highlight c++ linenos %}
#include "public.h"
using namespace std;
void func() {
  cout << "call func()" << endl;
}
void Student::print() {
  cout << "msg...." << endl;
}
{% endhighlight %}

### hello.cpp
{% highlight c++ linenos %}
#include <iostream>
#include "../tools/public.h"
using namespace std;
int main() {
  cout << "Hello world!\n";
  func();
  Student student;
  student.print();
}
{% endhighlight %}

### 產生靜態庫

進入tools目錄
```
$ cd tools
```
產生靜態庫
```
$ g++ -c -o libpublic.a public.cpp
```
### 使用靜態庫

#### 法1

回到上層
```
$ cd..
```
產生demo01執行檔，原始檔案有libpublic.a的靜態庫
```
$ g++ -o demo01 app/hello.cpp tools/libpublic.a
```
執行
```
$ ./demo01
```
#### 法2

語法
```
g++ -o 執行檔名 原始檔案1.cpp -L 靜態庫目錄位址 -l 庫名
```
```
$ g++ -o demo01 app/hello.cpp -L tools -l public
```

## 動態庫

語法
```
g++ -fPIC -shared -o lib庫名.so 來源檔案1.cpp
```
-fPIC -shared 這二句代表製作動態庫

-o 產生檔案

lib庫名.so lib與.so是固定寫法，庫名可以自已取名。

```
$ cd tools
$ g++ -fPIC -shared -o libpublic.so public.cpp
$ rm libpulic.a
```
為避免干擾，把libpublic.a靜態庫先刪掉

### 使用動態庫

將cpp檔案整理成一個庫，產生執行檔可以指定庫名.so，就不用寫很多原始檔案1.cpp 原始檔案2.cpp 原始檔案3.cpp 原始檔案N.cpp

動態庫是二進制檔案

多個procee用到同一個動態庫中的函式或類別，在記憶體中只有一份，多個process共享相同程式碼，也稱共享庫，因為只有一份程式碼，不會占用記憶體。

主程式在執行時，才把動態庫程式碼載入到記憶體。

優點，動態庫更新，有用到動態庫的程式不用重新編譯，只需要更新動態庫。

動態庫與靜態庫同時存在，優先使用動態庫。

#### 法1

```
$ cd ..
$ g++ -o demo01 app/hello.cpp tools/libpublic.so
$ ./demo01
```

#### 法2

先增加LD_LIBRARY_PATH環境變數，設定放置動態庫的目錄

```
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/cici/test/tools
```

```
$ g++ -o demo01 app/hello.cpp -L tools -l public
$ ./demo01
```
-L 動態庫目錄位址 -l 庫名

### 模擬更新動態庫

#### 將public.cpp的程式碼更新。

public.cpp
{% highlight c++ linenos %}
#include "public.h"
using namespace std;
void func() {
  cout << "update all func()" << endl;
}
void Student::print() {
  cout << "update msg...." << endl;
}
{% endhighlight %}

#### 重新建立動態庫
```
$ cd tools
$ g++ -fPIC -shared -o libpublic.so public.cpp
```

#### 執行主程式

```
$ cd ..
$ ./demo01
Hello world!
update call func()
update msg....
```

#### 總結

以上的過程，都不必重新編譯主程式。

## makefile

### 建立makefile
```
$ cd tools
$ vi makefile
```
#### 產生靜態庫,動態庫

用空格分開

```
all: libpublic.a libpublic.so
```

換行用\\

<span class="markline">注意！第二行是用tab，不是空格</span>
```
all: libpublic.a \
     libpublic.so
```

#### 動態庫或靜態庫中的cpp更新，重新編譯

編譯靜態庫libpublic.a，需要依賴public.h和public.cpp

若public.h或public.cpp更新，需要重新編譯

<span class="markline">注意！第二行是用tab，不是空格</span>
```
libpublic.a:public.h public.cpp
	g++ -c -o libpublic.a public.cpp
```

編譯動態庫libpublic.so，需要依賴public.h和public.cpp

若public.h或public.cpp更新，需要重新編譯
```
libpublic.so:public.h public.cpp
	g++ -fPIC -shared -o libpublic.so public.cpp
```

#### clean刪除編譯完的檔案
```
clean:
	rm -f libpublic.a libpublic.so
```

### 使用makefile

#### 清理

語法
```
make clean
```

```
cici@cici-vm:~/test/tools$ ls
libpublic.a  libpublic.so  makefile  public.cpp  public.h
cici@cici-vm:~/test/tools$ make clean
rm -f libpublic.a libpublic.so
cici@cici-vm:~/test/tools$ ls
makefile  public.cpp  public.h
```

#### 產生靜態庫,動態庫


語法
```
make
```

```
cici@cici-vm:~/test/tools$ make
g++ -c -o libpublic.a public.cpp
g++ -fPIC -shared -o libpublic.so public.cpp
cici@cici-vm:~/test/tools$ ls
libpublic.a  libpublic.so  makefile  public.cpp  public.h
```

若沒有修改任何.cpp或.h，再次make不會進行動作
```
cici@cici-vm:~/test/tools$ make
make: 對「all」無需做任何事。
```

修改public.cpp

public.cpp
{% highlight c++ linenos %}
#include "public.h"
using namespace std;
void func() {
  cout << "update2 all func()" << endl;
}
void Student::print() {
  cout << "update2 msg...." << endl;
}
{% endhighlight %}

若有更改.cpp，make就會重新編譯
```
cici@cici-vm:~/test/tools$ vi public.cpp
cici@cici-vm:~/test/tools$ make
g++ -c -o libpublic.a public.cpp
g++ -fPIC -shared -o libpublic.so public.cpp
```

若刪除靜態檔，再make，就會重新產生靜態檔
```
cici@cici-vm:~/test/tools$ rm libpublic.a
cici@cici-vm:~/test/tools$ make
g++ -c -o libpublic.a public.cpp
cici@cici-vm:~/test/tools$ ls
libpublic.a  libpublic.so  makefile  public.cpp  public.h
```

### 多動態庫使用

#### 建立myapi庫

回到最上層目錄，建立api的目錄，並在api目錄中，建立myapi.h

```
$ mkdir api
$ cd api
$ vi myapi.h
```

myapi.h
{% highlight c++ linenos %}
#include <iostream>
void func1();
class Teacher {
 public:
  void print();
};
{% endhighlight %}

myapi.cpp
{% highlight c++ linenos %}
#include "myapi.h"
using namespace std;
void func1() {
  cout << "call func1()" << endl;
}
void Teacher::print() {
  cout << "teacher ... msg ..." << endl;
}
{% endhighlight %}

進入api目錄，並建立makefile

{% highlight c++ linenos %}
all:libmyapi.a libmyapi.so
libmyapi.a:myapi.h myapi.cpp
	g++ -c -o libmyapi.a myapi.cpp
libmyapi.so:myapi.h myapi.cpp
	g++ -fPIC -shared -o libmyapi.so myapi.cpp
clean:
	rm -f libmyapi.a libmyapi.so
{% endhighlight %}

```
cici@cici-vm:~/test/api$ make
g++ -c -o libmyapi.a myapi.cpp
g++ -fPIC -shared -o libmyapi.so myapi.cpp
```

app目錄下的hello.cpp修改
{% highlight c++ linenos %}
#include <iostream>
#include "../tools/public.h"
#include "../api/myapi.h"
using namespace std;
int main() {
  cout << "Hello world!\n";
  func();
  Student student;
  student.print();
  //新增修改
  func1();
  Teacher teacher;
  teacher.print();
}
{% endhighlight %}

api目錄加到LD_LIBRARY_PATH
```
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/cici/test/api
```

回到test目錄下，執行以下語句，設定myapi動態檔放置的目錄(-L)與庫名(-l)
```
$ g++ -o demo01 app/hello.cpp -L tools -l public -L api -l myapi
$ ./demo01
```

```
Hello world!
update2 call func()
update2 msg....
call func1()
teacher ... msg ...
```

#### -I

修改hello.cpp的include public.h與myapi.h路徑。
{% highlight c++ linenos %}
#include <iostream>
#include "public.h"
#include "myapi.h"
using namespace std;
int main() {
  cout << "Hello world!\n";
  func();
  Student student;
  student.print();
  //新增修改
  func1();
  Teacher teacher;
  teacher.print();
}
{% endhighlight %}

使用-I，指定.h檔案的路徑

```
cici@cici-vm:~/test$ g++ -o demo01 app/hello.cpp -L tools -l public -L api -l myapi -I /home/cici/test/tools -I /home/cici/test/api
cici@cici-vm:~/test$ ./demo01
Hello world!
update2 call func()
update2 msg....
call func1()
teacher ... msg ...
```

#### 為demo01寫makefile

路徑如下
cici@cici-vm:~/test$ tree
.
├── api
│   ├── libmyapi.a
│   ├── libmyapi.so
│   ├── makefile
│   ├── myapi.cpp
│   └── myapi.h
├── app
│   ├── demo01
│   ├── hello
│   ├── hello.cpp
│   └── makefile
├── makefile
└── tools
    ├── libpublic.a
    ├── libpublic.so
    ├── makefile
    ├── public.cpp
    └── public.h

在/home/cici/test目錄下撰寫的makefile:
{% highlight c++ linenos %}
all:demo01
demo01:app/hello.cpp
        g++ -o demo01 app/hello.cpp -L /home/cici/test/tools -l public -L /home/cici/test/api -l myapi -I /home/cici/test/tools -I /home/cici/test/api
clean:
        rm -f demo01
{% endhighlight %}

在/home/cici/test/app目錄下撰寫的makefile:
{% highlight c++ linenos %}
all:demo01
demo01:hello.cpp
        g++ -o demo01 hello.cpp -L /home/cici/test/tools -l public -L /home/cici/test/api -l myapi -I /home/cici/test/tools -I /home/cici/test/api
clean:
        rm -f demo01
{% endhighlight %}

#### 多個檔案

在makefile定義變數
```
INCLUDEDIR=-I /home/cici/test/tools -I /home/cici/test/api
LIBDIR=-L /home/cici/test/tools -L /home/cici/test/api
```

使用變數
```
$(變數)
```

多個檔案
{% highlight c++ linenos %}
INCLUDEDIR=-I /home/cici/test/tools -I /home/cici/test/api
LIBDIR=-L /home/cici/test/tools -L /home/cici/test/api
all:demo01 demo02 demo03
demo01:hello.cpp
        g++ -o demo01 hello.cpp $(INCLUDEDIR) $(LIBDIR) -l public -l myapi
demo02:hello.cpp
        g++ -o demo02 hello.cpp $(INCLUDEDIR) $(LIBDIR) -l public -l myapi
demo03:hello.cpp
        g++ -o demo03 hello.cpp $(INCLUDEDIR) $(LIBDIR) -l public -l myapi
clean:
        rm -f demo01 demo02 demo03
{% endhighlight %}

```
$ vi makefile
$ make
g++ -o demo02 hello.cpp -I /home/cici/test/tools -I /home/cici/test/api -L /home/cici/test/tools -L /home/cici/test/api -l public -l myapi
g++ -o demo03 hello.cpp -I /home/cici/test/tools -I /home/cici/test/api -L /home/cici/test/tools -L /home/cici/test/api -l public -l myapi
$ ls
demo01  demo02  demo03  hello  hello.cpp  makefile
$ vi makefile
$ ./demo01
Hello world!
update2 call func()
update2 msg....
call func1()
teacher ... msg ...
$ ./demo02
Hello world!
update2 call func()
update2 msg....
call func1()
teacher ... msg ...
$ ./demo03
Hello world!
update2 call func()
update2 msg....
call func1()
teacher ... msg ...
$ make clean
rm -f demo01 demo02 demo03
$ ls
hello  hello.cpp  makefile
```