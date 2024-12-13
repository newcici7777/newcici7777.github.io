---
title: 取得錯誤訊息
date: 2024-12-12
keywords: c++, errno, strerror, perror
---

## errno

errno是C函式庫(libc)中的全域變數，記錄使用C函式庫過程中產生的錯誤編號

## strerror

strerror()用於取得errno的錯誤詳細訊息。

{% highlight c++ linenos %}
#include <iostream>
#include <cstring>  // strerror() 需要head file
#include <cerrno>  // errno全域變數的head file
using namespace std;
int main() {
  // 印出errno以及錯誤詳細訊息
  for (int i = 0; i < 110; i++) {
    cout << "errno = " << i << ", " << strerror(i) << endl;
  }
  return 0;
}
{% endhighlight %}
```
errno = 0, Undefined error: 0
errno = 1, Operation not permitted
errno = 2, No such file or directory
errno = 3, No such process
errno = 4, Interrupted system call
errno = 5, Input/output error
errno = 6, Device not configured
errno = 7, Argument list too long
errno = 8, Exec format error
errno = 9, Bad file descriptor
errno = 10, No child processes
errno = 11, Resource deadlock avoided
errno = 12, Cannot allocate memory
errno = 13, Permission denied
errno = 14, Bad address
errno = 15, Block device required
errno = 16, Resource busy
errno = 17, File exists
errno = 18, Cross-device link
errno = 19, Operation not supported by device
errno = 20, Not a directory
errno = 21, Is a directory
errno = 22, Invalid argument
errno = 23, Too many open files in system
errno = 24, Too many open files
errno = 25, Inappropriate ioctl for device
errno = 26, Text file busy
errno = 27, File too large
errno = 28, No space left on device
errno = 29, Illegal seek
errno = 30, Read-only file system
errno = 31, Too many links
errno = 32, Broken pipe
errno = 33, Numerical argument out of domain
errno = 34, Result too large
errno = 35, Resource temporarily unavailable
errno = 36, Operation now in progress
errno = 37, Operation already in progress
errno = 38, Socket operation on non-socket
errno = 39, Destination address required
errno = 40, Message too long
errno = 41, Protocol wrong type for socket
errno = 42, Protocol not available
errno = 43, Protocol not supported
errno = 44, Socket type not supported
errno = 45, Operation not supported
errno = 46, Protocol family not supported
errno = 47, Address family not supported by protocol family
errno = 48, Address already in use
errno = 49, Can't assign requested address
errno = 50, Network is down
errno = 51, Network is unreachable
errno = 52, Network dropped connection on reset
errno = 53, Software caused connection abort
errno = 54, Connection reset by peer
errno = 55, No buffer space available
errno = 56, Socket is already connected
errno = 57, Socket is not connected
errno = 58, Can't send after socket shutdown
errno = 59, Too many references: can't splice
errno = 60, Operation timed out
errno = 61, Connection refused
errno = 62, Too many levels of symbolic links
errno = 63, File name too long
errno = 64, Host is down
errno = 65, No route to host
errno = 66, Directory not empty
errno = 67, Too many processes
errno = 68, Too many users
errno = 69, Disc quota exceeded
errno = 70, Stale NFS file handle
errno = 71, Too many levels of remote in path
errno = 72, RPC struct is bad
errno = 73, RPC version wrong
errno = 74, RPC prog. not avail
errno = 75, Program version wrong
errno = 76, Bad procedure for program
errno = 77, No locks available
errno = 78, Function not implemented
errno = 79, Inappropriate file type or format
errno = 80, Authentication error
errno = 81, Need authenticator
errno = 82, Device power is off
errno = 83, Device error
errno = 84, Value too large to be stored in data type
errno = 85, Bad executable (or shared library)
errno = 86, Bad CPU type in executable
errno = 87, Shared library version mismatch
errno = 88, Malformed Mach-o file
errno = 89, Operation canceled
errno = 90, Identifier removed
errno = 91, No message of desired type
errno = 92, Illegal byte sequence
errno = 93, Attribute not found
errno = 94, Bad message
errno = 95, EMULTIHOP (Reserved)
errno = 96, No message available on STREAM
errno = 97, ENOLINK (Reserved)
errno = 98, No STREAM resources
errno = 99, Not a STREAM
errno = 100, Protocol error
errno = 101, STREAM ioctl timeout
errno = 102, Operation not supported on socket
errno = 103, Policy not found
errno = 104, State not recoverable
errno = 105, Previous owner died
errno = 106, Interface output queue is full
errno = 107, Unknown error: 107
errno = 108, Unknown error: 108
errno = 109, Unknown error: 109
```

### 程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>  // perror() head file
#include <cstring>  // strerror() 需要head file
#include <cerrno>  // errno全域變數的head file
#include <sys/stat.h>  // mkdir的head file
using namespace std;
int main() {
  int ret = mkdir("/tmp/aaa", 0777);
  cout << "ret = " << ret << endl;
  cout << errno << ":" << strerror(errno) << endl;
  return 0;
}
{% endhighlight %}

執行第1次成功
```
ret = 0
0:Success
```

執行第2次失敗
```
ret = -1
17:File exists
```

## perror

回傳系統錯誤訊息，比起strerror(errno)，參數不用代入errno，代入的參數為使用者自訂的前綴字。

include
```
#include <unistd.h>  // perror() head file
```

```
void perror ( const char * str )  
```
參數str

先輸出字串str，後面跟著一個冒號，然後是一個空格，然後才是錯誤訊息。

將先前的程式碼strerror改成perror
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <cstring>  // strerror() 需要head file
#include <cerrno>  // errno全域變數的head file
#include <sys/stat.h>  // mkdir的head file
using namespace std;
int main() {
  int ret = mkdir("/tmp/aaa", 0777);
  cout << "ret = " << ret << endl;
  // cout << errno << ":" << strerror(errno) << endl;
  perror("msg");
  return 0;
}
{% endhighlight %}
```
ret = -1
msg: File exists
```