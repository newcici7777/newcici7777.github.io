---
title: system與execl
date: 2024-12-13
keywords: c++, system, exec
---

system()與execl()可以執行bash指令，把bash指令作為參數傳給system()與execl()函式。

## include
```
#include <unistd.h> 
```

## system

語法
```
int system(const char *command)
```

參數 bash指令

傳回值

- 0:成功
- 非0: 可能失敗

不管傳回值是0與非0，都不會修改全域變數errno，所以使用strerror()或perror()都會傳回Success

### 傳回值0程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>  // system() head file
#include <cstring>   // strerror() 需要head file
#include <cerrno>    // errno全域變數的head file
using namespace std;
int main() {
  int ret = system("/bin/ls -l");
  cout << "ret = " << ret << endl;
  cout << errno << ":" << strerror(errno) << endl;
  return 0;
}
{% endhighlight %}
```
-rwxrwxr-x 1 cici cici  16688 12月 13 13:32 system_test
-rw-rw-r-- 1 cici cici    333 12月 13 13:32 system_test.cpp
ret = 0
0:Success
```

### 傳回值非0程式碼1

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>  // system() head file
#include <cstring>   // strerror() 需要head file
#include <cerrno>    // errno全域變數的head file
using namespace std;
int main() {
  int ret = system("/bin/laaaaa -l");
  cout << "ret = " << ret << endl;
  cout << errno << ":" << strerror(errno) << endl;
  return 0;
}
{% endhighlight %}
```
sh: 1: /bin/laaaaa: not found
ret = 32512
0:Success
```

### 傳回值非0程式碼2

若bash是執行其它程式碼，其它程式碼的return值非0，system()函式的傳回值也會是非0。

以下是被呼叫的程序system_call.cpp
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  cout << "被呼叫的程序: system_call" << endl;
  return 12;
} 
{% endhighlight %}

system_test.cpp
{% highlight c++ linenos %}
int main() {
  // 執行system_call
  int ret = system("/home/cici/test/app/system_call");
  cout << "ret = " << ret << endl;
  cout << errno << ":" << strerror(errno) << endl;
  return 0;
}
{% endhighlight %}

```
被呼叫的程序: system_call
ret = 3072
0:Success
```

可以看出ret傳回值為非0，errno也是0，代表Success

## execl

### 語法
```
int execl(const char *path, const char *arg, ..., nullptr);
```

### 使用範例1
{% highlight c++ linenos %}
int ret = execl("/home/cici/system_call", "/home/cici/system_call", nullptr);
{% endhighlight %}
前2個參數都是執行檔案路徑，都是相同，最後一個參數為nullptr，代表之後沒有參數了。

### 使用範例2
{% highlight c++ linenos %}
// 呼叫/bin/ls -l /tmp
int ret = execl("/bin/ls", "/bin/ls", "-l", "/tmp", nullptr);
{% endhighlight %}
前2個參數都是bash指令執行檔路徑，都是相同，之後為bash指令的參數，最後一個參數為nullptr，代表之後沒有參數了。

### 程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>  // exec() head file
#include <string.h>
#include <cstring>   // strerror() 需要head file
#include <cerrno>    // errno全域變數的head file
using namespace std;
int main() {
  // 執行execl
  int ret = execl("/home/cici/test/app/system_call", "/home/cici/test/app/system_call", nullptr);
  cout << "ret = " << ret << endl;
  cout << errno << ":" << strerror(errno) << endl;
  return 0;
}
{% endhighlight %}
```
被呼叫的程序: system_call
```

從以上的執行結果可以發現，不會執行以下二行，因為程序已經被system_call程式碼取代
{% highlight c++ linenos %}
  cout << "ret = " << ret << endl;
  cout << errno << ":" << strerror(errno) << endl;
{% endhighlight %}

execl 函數用於在目前的 process 中啟動一個新的程式，取代目前 process 的執行。

也就是成功執行後，原來的程式碼將不再執行，因為目前 process 已被新程式取代。

如果要繼續執行原本的程式，可以使用 fork 讓 execl 在 child process 中執行

使用`ps -ef | grep execl`，會發現從頭到尾只有一個pid

但若是執行的是system()，會發現有2個pid

以下二個程式碼增加getpid()

execl_test.cpp
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>  // exec() head file
#include <string.h>
#include <cstring>   // strerror() 需要head file
#include <cerrno>    // errno全域變數的head file
using namespace std;
int main() {
  cout << "execl_test pid = " << getpid() << endl;
  // 執行execl
  int ret = execl("/home/cici/test/app/system_call", "/home/cici/test/app/system_call", nullptr);
  cout << "ret = " << ret << endl;
  cout << errno << ":" << strerror(errno) << endl;
  return 0;
}
{% endhighlight %}

system_call.cpp
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>  // getpid() head file
using namespace std;
int main() {
  cout << "system_call pid = " << getpid() << endl;
  cout << "被呼叫的程序: system_call" << endl;
  return 12;
}
{% endhighlight %}

編譯二個檔案，並執行execl_test

由執行結果可以發現，二個執行檔的pid都一模一樣。
```
execl_test pid = 131087
system_call pid = 131087
被呼叫的程序: system_call
```

## fork與execl

- [fork][1]

為了解決execl會取代主程序，使用fork()建立子程序，由子程序被execl取代，就不會取代主程序。
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <string.h>
#include <cstring>   // strerror() 需要head file
#include <cerrno>    // errno全域變數的head file
using namespace std;
int main() {
  // fork傳回值pid
  pid_t pid = fork();
  if (pid > 0) {
    // 父程序
    for (int i = 0; i < 15; i++) {
      sleep(1);
      cout << "第" << i << "秒" << endl;
    }
  } else {
    int ret = execl("/home/cici/test/app/system_call", "/home/cici/test/app/system_call", nullptr);
    cout << "ret = " << ret << endl;
    cout << errno << ":" << strerror(errno) << endl;
  }
  cout << "結束" << endl;
}
{% endhighlight %}
```
system_call pid = 146493
被呼叫的程序: system_call
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
結束
```

[1]: {% link _pages/c/libc/fork.md %}