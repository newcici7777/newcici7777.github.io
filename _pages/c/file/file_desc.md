---
title: File description
date: 2025-01-03
keywords: c++, File description
---
## 寫入文字檔
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <fcntl.h>  // open()要用到
#include <cstring>  // strcpy()要用到
using namespace std;
int main() {
  int fd;  // File descriptor
  // open file
  // O_CREAT 建立文字檔，如果檔案不存在就建立
  // O_RDWR 有讀與寫的權限
  // O_TRUNC 每一次寫入都直接把原檔內容清空覆蓋
  fd = open("/home/cici/test/app/test.txt", O_CREAT | O_RDWR | O_TRUNC);
  // 檔案操作失敗都會傳回-1，網路相關操作也是傳回-1
  if (fd == -1) {
    perror("open fail");
    return -1;
  }
  cout << "fd = " << fd << endl;
  // 寫入文字
  char buffer[1024];
  strcpy(buffer, "input data test1 ...");
  // write()函式參數用到fd(File descriptor)
  if (write(fd, buffer, strlen(buffer)) == -1) {
    perror("write fail");
    return -1;
  }
  // close()函式參數用到fd(File descriptor)
  close(fd);
  // 暫停100秒，方便查看proc中的fd目錄
  sleep(100);
  return 0;
}
{% endhighlight %}
```
fd = 3
```

## 讀取文字檔
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <fcntl.h>  // open()要用到
#include <cstring>  // strcpy()要用到
using namespace std;
int main() {
  int fd;  // File descriptor
  // open file
  // O_RDONLY 只有讀權限
  fd = open("/home/cici/test/app/test.txt", O_RDONLY);
  // 檔案操作失敗都會傳回-1，網路相關操作也是傳回-1
  if (fd == -1) {
    perror("open fail");
    return -1;
  }
  cout << "fd = " << fd << endl;
  // 讀取文件
  // 存放文件內容的字元陣列
  char buffer[1024];
  memset(buffer, 0, sizeof(buffer));
  // read()函式參數用到fd(File descriptor)
  if (read(fd, buffer, sizeof(buffer)) == -1) {
    perror("read fail");
    return -1;
  }
  cout << "檔案內容 = " << buffer << endl;
  // close()函式參數用到fd(File descriptor)
  close(fd);
  // 暫停100秒，方便查看proc中的fd目錄
  sleep(100);
  return 0;
}
{% endhighlight %}
```
$ ./fd_r_test
fd = 3
檔案內容 = input data test1 ...
```

## fd

在linux編譯執行以上程式碼

進入\/proc\/進程id\/fd目錄，裡面存放所有進程的File description

```
cd /proc/進程id/fd
```

```
$ cd /proc/13786
$ cd fd
$ ls
0  1  2
```

fd預設有
- 0 cin鍵盤輸入
- 1 cout輸出到瑩幕
- 2 cerr錯誤訊息輸出到瑩幕

### 關掉fd

使用close，可以關掉fd
```
close(fd);
```

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <fcntl.h>  // open()要用到
#include <cstring>  // strcpy()要用到
using namespace std;
int main() {
  // 關閉cin
  close(0);
  // 關閉cout
  //close(1);
  // 關閉cerr
  //close(2);
  char input[100];
  cin >> input;
  cout << "input = " << input << endl;
  cerr << "error!" << endl;
  // 暫時休眠200秒，方便使用proc查看fd
  sleep(200);
  return 0;
}
{% endhighlight %}

編譯後執行
```
$ ./input_test
input = (??6?
error!
```

開另一個終端機，進入fd的目錄查看，發現少一個0
```
/proc/13900/fd$ ls
1  2
```

若把0,1,2都關掉，fd的編號就會從0開始。
