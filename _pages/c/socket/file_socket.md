---
title: Socket傳送檔案
date: 2025-01-17
keywords: c++, socket send file
---
Prerequisites:
- [File IO][1]

## client

### 傳送流程

傳送檔名與大小 -> 取得server回應(確認server已收到檔名與大小) -> 傳送檔案 -> 取得server回應(確認server已收到檔案)

### 傳送檔名與檔案大小
{% highlight c++ linenos %}
  // 傳送檔名與大小
  bool send(void *buffer, const size_t size) {
    // client_fd若已斷線，直接返回
    if (client_fd_ == -1) return false;
    // send()第2個參數填記憶體位址，第3個參數填大小
    if ((::send(client_fd_, buffer, size, 0)) <= 0)
      return false;
    return true;
  }
{% endhighlight %}

### 傳送檔案
使用while無限迴圈，待檔案傳送完，就會離開無限迴圈。

把每個byte都視為1個磚頭，有1000個磚頭，每次只搬7個磚頭，直至1000個磚頭搬送完畢。

檔案每次只傳7byte，除非剩下要傳的byte小於7byte，才用小於7byte的數字傳送。

了解原理後，可以把7byte改成1024byte，每次只傳1024byte，直到全部的byte都傳送完畢。

{% highlight c++ linenos %}
  // 傳送檔案
  bool sendFile(const string &file_name, const size_t file_size) {
    // 參數2指定binaryfile
    ifstream fin(file_name, ios::binary);
    if (fin.is_open() == false) {
      cout << "open file (" << file_name << ") fail" << endl;
      return false;
    }
    // 每次要讀資料的個數(byte)
    int read = 0;
    // 記錄已傳送資料的個數，若total_byte == file_size代表傳送完
    int total_bytes = 0;
    // 每次只傳送7byte
    char buffer[7];
    while (true) {
      // 清空
      memset(buffer, 0, sizeof(buffer));
      // 如果剩下的byte數量，大於7個，就傳送7個
      if (file_size - total_bytes > 7)
        read = 7;
      else
        //小於7，就傳小於7的數量
        read = file_size - total_bytes;
      fin.read(buffer, read);
      // 把讀到的資料送給server
      if (send(buffer, read) == false) return false;
      // 加上已傳送的數量
      total_bytes += read;
      if (total_bytes == file_size) break;
    }
    return true;
  }
{% endhighlight %}

### main參數
改為傳送5個參數，`./client_test ip port 檔名 檔案大小`

{% highlight c++ linenos %}
  if (argc != 5) {
    cout << "請輸入 ./client_test ip port 檔名 檔案大小" << endl;
    return -1;
  }
{% endhighlight %}

### 檔案結構

包含檔名與檔案大小

{% highlight c++ linenos %}
  // 檔案結構
  struct FileInfo {
    char filename[256];  // 檔名
    int filesize;  // 檔案大小
  } file_info;  // 變數名
  // 清空
  memset(&file_info, 0, sizeof(file_info));
  strcpy(file_info.filename, argv[3]);  // 檔名
  file_info.filesize = atoi(argv[4]);  // 檔案大小  
{% endhighlight %}

### 傳送檔名與檔案大小

{% highlight c++ linenos %}
  // 傳送檔名與檔案大小
  if (tcp_client.send(&file_info, sizeof(file_info)) == false) {
    perror("send");
    return -1;
  }
{% endhighlight %}  

### 傳送完檔名與檔案後，取得server回應

傳送完檔名與檔案大小，要等待server回應"ok"，代表server收到檔名與大小。

{% highlight c++ linenos %}
  // 收到server回應
  string buffer;
  // 參數2，"ok"是2byte
  if (tcp_client.recv(buffer, 2) == false) {
    perror("recv");
    return -1;
  }
  if (buffer != "ok") {
    cout << "server沒回應ok" << endl;
    return -1;
  }
{% endhighlight %}  

### 呼叫傳送檔案

{% highlight c++ linenos %}
  // 傳送檔案
  if (tcp_client.sendFile(file_info.filename, file_info.filesize)) {
    perror("sendFile");
    return -1;
  }
{% endhighlight %}  

### 傳送檔案後，取得server回應
{% highlight c++ linenos %}
  // 參數2，"ok"是2byte
  if (tcp_client.recv(buffer, 2) == false) {
    perror("recv");
    return -1;
  }
  if (buffer != "ok") {
    cout << "傳送檔案失敗" << endl;
    return -1;
  }
  cout << "client 傳送檔案成功" << endl;
{% endhighlight %} 

## server

### 接收檔名與大小
{% highlight c++ linenos %}
  // 收到client傳來的檔案結構
  bool recv(void* buffer, const size_t size) {
    if (::recv(client_fd_, buffer, size, 0) <= 0)
      return false;
    return true;
  }
{% endhighlight %}

### 接收檔案
{% highlight c++ linenos %}  
  bool recvFile(const string &file_name, const size_t file_size) {
    ofstream fout;
    fout.open(file_name, ios::binary);
    if (fout.is_open() == false) {
      cout << "open file " << file_name << "fail" << endl;
      return false;
    }
    int total_bytes = 0;
    int read = 0;
    char buffer[7];
    while (true) {
      if (file_size - total_bytes > 7) {
        read = 7;
      } else {
        read = file_size - total_bytes;
      }
      if (recv(buffer, read) == false)
        return false;
      fout.write(buffer, read);
      total_bytes += read;
      if (total_bytes == file_size) break;
    }
    return true;
  }
{% endhighlight %}

### main參數

改為傳送3個參數，`./server_test port 存放目錄`

{% highlight c++ linenos %}  
  if (argc != 3) {
    cout << "請輸入 ./server_test port 存放目錄" << endl;
    return -1;
  }
{% endhighlight %}

### 檔案結構

{% highlight c++ linenos %}  
  // 檔案結構
  struct FileInfo {
    char filename[256];  // 檔名
    int filesize;  // 檔案大小
  } file_info;  // 變數名
  // 清空
  memset(&file_info, 0, sizeof(file_info));
{% endhighlight %}

### 接收檔名與大小

{% highlight c++ linenos %}  
  if (tcpServer.recv(&file_info, sizeof(file_info)) == false) {
    perror("recv()");
    return -1;
  }
  cout << "server 收到檔名 = " << file_info.filename <<
  ", 檔案大小 = " << file_info.filesize << endl;
{% endhighlight %}

### 回應client已收到檔名與大小

{% highlight c++ linenos %} 
  if (tcpServer.send("ok") == false) {
    perror("send");
    return -1;
  }
{% endhighlight %}

### 接收檔案

{% highlight c++ linenos %}
  if (tcpServer.recvFile(string(argv[2]) + "/" + file_info.filename, file_info.filesize) == false) {
    cout << "檔案傳送失敗" << endl;
    return -1;
  }
  cout << "檔案傳送成功" << endl;
{% endhighlight %}

### 回應client已收到檔案
{% highlight c++ linenos %}
tcpServer.send("ok");
{% endhighlight %}

## 完整程式碼

### client程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <cstring>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <fstream>
using namespace std;
class TCPClient {
 public:
  // 建構子，初始化client_fd，-1代表沒有連線
  TCPClient() : client_fd_(-1) {}
  bool connect(const string &ip, const unsigned short port) {
    // client_fd若已連線，就不用再連，直接返回
    if (client_fd_ != -1) return false;
    ip_ = ip;
    port_ = port;
    client_fd_ = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
    // socket失敗傳回-1
    if (client_fd_ == -1)
      return false;
    // server ip轉big-endian
    struct hostent* h;
    // gethostbyname需要cstring，所以要把string轉成cstring
    if ((h = gethostbyname(ip_.c_str())) == nullptr) {
      cout << "gethostbyname() fail." << endl;
      // 關閉socket
      ::close(client_fd_);
      // 失敗把client_fd還原成初始化-1
      client_fd_ = -1;
      return false;
    }
    // sockaddr_in結構
    struct sockaddr_in server_addr;
    memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    // server ip
    memcpy(&server_addr.sin_addr, h->h_addr, h->h_length);
    // server port
    server_addr.sin_port = htons(port_);
    // connect伺服器，傳回值為0代表連線成功，不是0代表連線失敗
    if (::connect(client_fd_, (struct sockaddr*)&server_addr, sizeof(server_addr)) != 0) {
      // 關閉socket
      ::close(client_fd_);
      // 失敗把client_fd還原成初始化-1
      client_fd_ = -1;
      return false;
    }
    return true;
  }
  // 參數使用const string是因為既可支援string，也可支援const char*
  bool send(const string &buffer) {
    // client_fd若已斷線，直接返回
    if (client_fd_ == -1) return false;
    // send()第2個參數填記憶體位址，第3個參數填大小
    if ((::send(client_fd_, buffer.data(), buffer.size(), 0)) <= 0)
      return false;
    return true;
  }
  // 傳送檔名與大小
  bool send(void *buffer, const size_t size) {
    // client_fd若已斷線，直接返回
    if (client_fd_ == -1) return false;
    // send()第2個參數填記憶體位址，第3個參數填大小
    if ((::send(client_fd_, buffer, size, 0)) <= 0)
      return false;
    return true;
  }
  // 傳送檔案
  bool sendFile(const string &file_name, const size_t file_size) {
    // 參數2指定binaryfile
    ifstream fin(file_name, ios::binary);
    if (fin.is_open() == false) {
      cout << "open file (" << file_name << ") fail" << endl;
      return false;
    }
    // 每次要讀資料的個數(byte)
    int read = 0;
    // 記錄已傳送資料的個數，若total_byte == file_size代表傳送完
    int total_bytes = 0;
    // 每次只傳送7byte
    char buffer[7];
    while (true) {
      // 清空
      memset(buffer, 0, sizeof(buffer));
      // 如果剩下的byte數量，大於7個，就傳送7個
      if (file_size - total_bytes > 7)
        read = 7;
      else
        //小於7，就傳小於7的數量
        read = file_size - total_bytes;
      fin.read(buffer, read);
      // 把讀到的資料送給server
      if (send(buffer, read) == false) return false;
      // 加上已傳送的數量
      total_bytes += read;
      if (total_bytes == file_size) break;
    }
    return true;
  }
  // 第一個參數要修改buffer的內容，所以不加const
  bool recv(string &buffer, const size_t maxlen) {
    // 清空string
    buffer.clear();
    // 分配記憶體大小
    buffer.resize(maxlen);
    // 收到的資料放在buffer的記憶體空間
    // 操作到string的記憶體位址，有對string物件進行讀取與寫入
    // 第2個參數填不是const的記憶體位址，第3個參數填大小
    // &buffer[0]取得位址，傳回值不是const
    // buffer.data()傳回值是const，所以不用
    // recv()傳回值是收到的資料大小
    int iret = ::recv(client_fd_, &buffer[0], buffer.size(), 0);
    // iret = -1 失敗
    // iret = 0 socket斷線
    // iret > 0 收到資料
    if (iret <= 0) {
      // 失敗時把buffer清空，否則buffer大小一直維持1024
      buffer.clear();
      return false;
    }
    // 如果有操作到string的記憶體位址，string的自動擴展空間會失效
    // 需要自己重設string大小
    // 從1024大小，設為真正收到資料的大小
    buffer.resize(iret);
    return true;
  }
  bool close() {
    // client_fd若已斷線，直接返回
    if (client_fd_ == -1) return false;
    ::close(client_fd_);
    client_fd_ = -1;
    return true;
  }
  ~TCPClient() {
    close();
  }
 private:
  int client_fd_;
  string ip_;
  unsigned short port_;
};
int main(int argc, char *argv[]) {
  if (argc != 5) {
    cout << "請輸入 ./client_test ip port 檔名 檔案大小" << endl;
    return -1;
  }
  // 建立socket
  TCPClient tcp_client;
  if (tcp_client.connect(argv[1], atoi(argv[2])) == false) {
    perror("socket");
    return -1;
  }
  // 檔案結構
  struct FileInfo {
    char filename[256];  // 檔名
    int filesize;  // 檔案大小
  } file_info;  // 變數名
  // 清空
  memset(&file_info, 0, sizeof(file_info));
  strcpy(file_info.filename, argv[3]);  // 檔名
  file_info.filesize = atoi(argv[4]);  // 檔案大小
  // 傳送結構(檔名與檔案大小)
  if (tcp_client.send(&file_info, sizeof(file_info)) == false) {
    perror("send");
    return -1;
  }
  cout << "傳送檔名 = " << file_info.filename <<
  ", 檔案大小 = " << file_info.filesize << endl;
  // 收到server回應
  string buffer;
  // 參數2，"ok"是2byte
  if (tcp_client.recv(buffer, 2) == false) {
    perror("recv");
    return -1;
  }
  if (buffer != "ok") {
    cout << "server沒回應ok" << endl;
    return -1;
  }
  // 傳送檔案
  if (tcp_client.sendFile(file_info.filename, file_info.filesize)) {
    perror("sendFile");
    return -1;
  }
  // 參數2，"ok"是2byte
  if (tcp_client.recv(buffer, 2) == false) {
    perror("recv");
    return -1;
  }
  if (buffer != "ok") {
    cout << "傳送檔案失敗" << endl;
    return -1;
  }
  cout << "client 傳送檔案成功" << endl;
  return 0;
}
{% endhighlight %}

### server程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <cstring>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <fstream>
using namespace std;
class TCPServer {
 public:
  // 建構子
  // 監聽listen_fd，-1代表未初始化
  // client_fd，-1代表未連線
  TCPServer() : listen_fd_(-1), client_fd_(-1) {}
  // 初始化監聽的socket
  bool initListen(const unsigned short port) {
    listen_fd_ = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
    if (listen_fd_ == -1) return false;
    port_ = port;
    // sockaddr_in結構
    struct sockaddr_in server_addr;
    memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    // server port
    server_addr.sin_port = htons(port_);
    // server有多個ip，所有ip都會監聽port
    server_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    // socket綁定ip與port
    if (::bind(listen_fd_, (struct sockaddr*)&server_addr, sizeof(server_addr)) == -1) {
      // 關閉監聽器
      close(listen_fd_);
      listen_fd_ = -1;
      return false;
    }
    // 把socket設為監聽的狀態
    // 最多只讓3個client同時連server
    if (listen(listen_fd_, 3)) {
      close(listen_fd_);
      listen_fd_ = -1;
      return false;
    }
    return true;
  }
  bool accept() {
    // client的ip結構
    struct sockaddr_in client_addr;
    socklen_t addr_len = sizeof(client_addr);
    // 接受client的連線
    // 若要得到client的ip，第二個參數填sockaddr_in結構，第3個參數填結構大小
    // 若不想得到client ip，第2與第3參數填0就好
    client_fd_ = ::accept(listen_fd_, (struct sockaddr*)&client_addr, &addr_len);
    if (client_fd_ == -1) return false;
    // 取得client的ip
    // 透過inet_ntoa把big-endian轉成little-endian
    client_ip_ = inet_ntoa(client_addr.sin_addr);
    return true;
  }
  const string& getClientIp() const {
    return client_ip_;
  }
  bool send(const string &buffer) {
    // 如果client fd是-1就沒必要繼續後面的動作，直接返回
    if (client_fd_ == -1) return false;
    // send()第2個參數填記憶體位址，第3個參數填大小
    if ((::send(client_fd_, buffer.data(), buffer.size(), 0)) <= 0)
      return false;
    return true;
  }
  // 第一個參數要修改buffer的內容，所以不加const
  bool recv(string &buffer, const size_t maxlen) {
    // 清空string
    buffer.clear();
    // 分配記憶體大小
    buffer.resize(maxlen);
    // 收到的資料放在buffer的記憶體空間
    // 操作到string的記憶體位址，有對string物件進行讀取與寫入
    // 第2個參數填不是const的記憶體位址，第3個參數填大小
    // &buffer[0]取得位址，傳回值不是const
    // buffer.data()傳回值是const，所以不用
    // recv()傳回值是收到的資料大小
    int iret = ::recv(client_fd_, &buffer[0], buffer.size(), 0);
    // iret = -1 失敗
    // iret = 0 socket斷線
    // iret > 0 收到資料
    if (iret <= 0) {
      cout << "iret = " << iret << endl;
      // 失敗時把buffer清空，否則buffer大小一直維持1024
      buffer.clear();
      return false;
    }
    // 如果有操作到string的記憶體位址，string的自動擴展空間會失效
    // 需要自己重設string大小
    // 從1024大小，設為真正收到資料的大小
    buffer.resize(iret);
    return true;
  }
  // 收到client傳來的檔案結構
  bool recv(void* buffer, const size_t size) {
    if (::recv(client_fd_, buffer, size, 0) <= 0)
      return false;
    return true;
  }
  bool recvFile(const string &file_name, const size_t file_size) {
    ofstream fout;
    fout.open(file_name, ios::binary);
    if (fout.is_open() == false) {
      cout << "open file " << file_name << "fail" << endl;
      return false;
    }
    int total_bytes = 0;
    int read = 0;
    char buffer[7];
    while (true) {
      if (file_size - total_bytes > 7) {
        read = 7;
      } else {
        read = file_size - total_bytes;
      }
      if (recv(buffer, read) == false)
        return false;
      fout.write(buffer, read);
      total_bytes += read;
      if (total_bytes == file_size) break;
    }
    return true;
  }
  // 關閉監聽socket
  bool closeListen() {
    // 若未初始化，直接返回
    if (listen_fd_ == -1) return false;
    ::close(listen_fd_);
    // 設為未初始化-1
    listen_fd_ = -1;
    return true;
  }
  // 關閉client socket
  bool closeClient() {
    // client_fd若已斷線，直接返回
    if (client_fd_ == -1) return false;
    ::close(client_fd_);
    // 設成未連線-1
    client_fd_ = -1;
    return true;
  }
  ~TCPServer() {
    // 解構子把二個socket都關掉
    closeListen();
    closeClient();
  }
 private:
  int listen_fd_;
  int client_fd_;
  string client_ip_;
  unsigned short port_;
};
int main(int argc, char *argv[]) {
  if (argc != 3) {
    cout << "請輸入 ./server_test port 存放目錄" << endl;
    return -1;
  }
  // 建立監聽fd listen_fd
  TCPServer tcpServer;
  // 初始化監聽
  if (tcpServer.initListen(atoi(argv[1])) == false) {
    perror("initListen");
    return -1;
  }
  // 接受client連接
  if (tcpServer.accept() == false) {
    perror("accept");
    return -1;
  }
  cout << "client已連上 = " << tcpServer.getClientIp() << endl;
  // 檔案結構
  struct FileInfo {
    char filename[256];  // 檔名
    int filesize;  // 檔案大小
  } file_info;  // 變數名
  // 清空
  memset(&file_info, 0, sizeof(file_info));
  if (tcpServer.recv(&file_info, sizeof(file_info)) == false) {
    perror("recv()");
    return -1;
  }
  cout << "server 收到檔名 = " << file_info.filename <<
  ", 檔案大小 = " << file_info.filesize << endl;
  if (tcpServer.send("ok") == false) {
    perror("send");
    return -1;
  }
  if (tcpServer.recvFile(string(argv[2]) + "/" + file_info.filename, file_info.filesize) == false) {
    cout << "檔案傳送失敗" << endl;
    return -1;
  }
  cout << "檔案傳送成功" << endl;
  tcpServer.send("ok");
  return 0;
}
{% endhighlight %}

### 測試方式

#### server
```
$ ./server_test 1234 /tmp
client已連上 = 192.168.235.128
server 收到檔名 = test1.txt, 檔案大小 = 15890
檔案傳送成功
$ cd /tmp
$ ls
test1.txt
VMwareDnD
vmware-root_727-4290690966
$ ll test1.txt
-rw-rw-r-- 1 cici cici 15890  1月 17 13:03 test1.txt
```

#### client
```
$ ll test1.txt
-rw-rw-r-- 1 cici cici 15890 12月 31 13:33 test1.txt
$ ./client_test 192.168.235.128 1234 test1.txt 15890
傳送檔名 = test1.txt, 檔案大小 = 15890
sendFile: Success
```

[1]: {% link _pages/c/file/file_io.md %}