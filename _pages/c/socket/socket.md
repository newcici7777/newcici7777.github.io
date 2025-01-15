---
title: socket
date: 2025-01-12
keywords: c++, Socket
---
Prerequisites:
- [File description][1]
- [errno][2]
- [main][3]

## socket()函式
{% highlight c++ linenos %}
int socket(int domain, int type, int protocol);
{% endhighlight %}
- 失敗傳回-1，錯誤訊息已放至[errno][2]
- 成功傳回[fd][1]

### 參數domain

固定用AF_INET

AF_INET支援TCP與UDP通訊傳輸協定。

### 參數type

|SOCK_STREAM|使用在TCP可靠傳輸協議，封包不會丟失|
|SOCK_DGRAM|使用在UDP不可靠傳輸協議，封包會丟失|

### 參數protocol

#### IPPROTO_TCP
建立TCP socket語法
{% highlight c++ linenos %}
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
{% endhighlight %}

#### IPPROTO_UDP
建立UDP socket語法
{% highlight c++ linenos %}
socket(AF_INET, SOCK_DGRAM, IPPROTO_UDP); 
{% endhighlight %}

## big-endian
網路傳輸以big-endian方式傳輸。

有些電腦是以little-endian方式進行記憶體儲存，但在網路傳輸，一律轉成big-Endian。

host，主機，伺服器提供網站服務，此時這個伺服器是主機。

## port轉成big-endian
透過以下語法把port互轉為big-Endian與little-endian
{% highlight c++ linenos %}
// port轉成big-endian
uint16_t h to n s(uint16_t hostshort);  // uint16_t 2byte unsigned short
uint32_t htonl(uint32_t hostlong);      // uint32_t 4byte unsigned int
// big-endian的port轉成little-endian
uint16_t n to h s(uint16_t netshort);
uint32_t ntohl(uint32_t netlong);
{% endhighlight %}

h = host(主機)

to = 轉

n = network(網路)

s = short(2byte=16bit)

l = long(4byte=32bit)

## sockaddr結構
{% highlight c++ linenos %}
struct sockaddr {
  unsigned short sa_family;  // 固定填AF_INET
  unsigned char sa_data[14]; // 14byte的ip與port
};
{% endhighlight %}

## sockaddr_in結構
大小與sockaddr結構一模一樣，但sockaddr結構的sa_data[14]範圍太大，此sockaddr_in結構補足sockaddr的不足。
{% highlight c++ linenos %}
struct sockaddr_in {  
  unsigned short sin_family;  // 固定填AF_INET
  unsigned short sin_port;    // 16bit,2byte port number,用htons()函式轉成big-endian
  struct in_addr sin_addr;	  // ip結構32bit,4byte
  unsigned char sin_zero[8];  // 未使用，主要補足14byte - (ip 4byte) - (port 2byte) = 8 byte
};
// ip結構
struct in_addr {
  unsigned int s_addr;  // 32bit,4byte ip 透過gethostbyname()函式轉成big-endian
};
{% endhighlight %}

在呼叫connect()函式時，再把sockaddr_in結構轉成sockaddr結構，二者記憶體大小都一樣，可以轉換。
{% highlight c++ linenos %}
(struct sockaddr* )&server_addr
{% endhighlight %}

## ip轉成big-endian
gethostbyname()函式將ip轉成big-endian
{% highlight c++ linenos %}
struct hostent* gethostbyname(const char* name);
{% endhighlight %}

- [define][4]

hostent結構
{% highlight c++ linenos %}
struct hostent { 
  char* h_name;      // 主機名
  char** h_aliases;  // 主機別名
  short h_addrtype;  // ip類型，固定填AF_INET
  short h_length;    // ip長度, ipv4 是4 byte
  char** h_addr_list;  //ip address以big-endian存放
};
#define h_addr h_addr_list[0]  // h_addr是h_addr_list[0]的別名
{% endhighlight %}

透過以下語法將轉成big-endian的ip拷貝到sockaddr_in結構
{% highlight c++ linenos %}
memcpy(&sockaddr_in.sin_addr,h->h_addr,h->h_length);
{% endhighlight %}

## client建立socket
建立socket流程如下

socket()函式 -> 轉換ip與port變成big-endian -> connect() -> send()

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <cstring>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;
int main(int argc, char *argv[]) {
  if (argc != 3) {
    cout << "請輸入 ./client_test ip port" << endl;
    return -1;
  }
  // 建立socket，失敗傳回-1
  int socket_fd = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
  if (socket_fd == -1) {
    perror("socket");
    return -1;
  }
  // server ip轉big-endian
  struct hostent* h;
  if ((h = gethostbyname(argv[1])) == nullptr) {
    cout << "gethostbyname() fail." << endl;
    // 關閉socket
    close(socket_fd);
    return -1;
  }
  // sockaddr_in結構
  struct sockaddr_in server_addr;
  memset(&server_addr, 0, sizeof(server_addr));
  server_addr.sin_family = AF_INET;
  // server ip
  memcpy(&server_addr.sin_addr, h->h_addr, h->h_length);
  // server port
  server_addr.sin_port = htons(atoi(argv[2]));
  // connect伺服器，傳回值為0代表連線成功，不是0代表連線失敗
  if (connect(socket_fd, (struct sockaddr*)&server_addr, sizeof(server_addr)) != 0) {
    perror("connect");
    // 關閉socket
    close(socket_fd);
    return -1;
  }
  // send data給伺服器
  char buffer[1024];
  for (int i = 0; i < 3; i++) {
    // 宣告send()與recv()的傳回值
    int iret;
    // 清空buffer
    memset(buffer, 0, sizeof(buffer));
    // sprintf 格式化輸出到 buffer 裡。
    sprintf(buffer, "data %d", i + 1);
    // send buffer to server
    if ((iret = send(socket_fd, buffer, strlen(buffer), 0)) <= 0) {
      perror("send");
      break;
    }
    cout << "傳送給伺服器 = " << buffer << endl;
    // 清空buffer
    memset(buffer, 0, sizeof(buffer));
    // 收伺服器的回應
    if ((iret = recv(socket_fd, buffer, sizeof(buffer), 0)) <= 0) {
      cout << "iret = " << iret << endl;
      break;
    }
    cout << "伺服器的回應 = " << buffer << endl;
    //暫停1秒
    sleep(1);
  }
  close(socket_fd);
  return 0;
}
{% endhighlight %}

## server

### systemctl
systemctl指令是對服務進行管理

語法
```
$ sudo systemctl 操作 服務名
```

以下為開啟服務，停止服務，重啟服務，查看服務狀態，檢查是否啟動
```
$ sudo systemctl start 服務名
$ sudo systemctl stop 服務名
$ sudo systemctl restart 服務名
$ sudo systemctl status 服務名
$ sudo systemctl is-active 服務名
```

Linux重新開機後會自動啟動服務
```
$ sudo systemctl enable 服務名
```

Linux重新開機後會關閉自動啟動服務
```
$ sudo systemctl disable 服務名
```

查看是否為開始自動啟動服務
```
$ sudo systemctl is-enabled 服務名
```

### 防火牆
以下步驟是更新 -> 安裝防火牆 -> 啟動防火牆 -> 查看防火牆狀態為active(啟動)
```
$ sudo apt-get update
$ sudo apt-get install ufw
$ sudo systemctl start ufw
$ sudo systemctl status ufw
● ufw.service
     Loaded: not-found (Reason: Unit ufw.service not found.)
     Active: active (exited) since Wed 2025-01-15 09:24:41 CST; 8min ago
   Main PID: 31048 (code=exited, status=0/SUCCESS)
        CPU: 501ms
```

### open listen port
增加對外開放的port
```
$ sudo ufw allow 1234
$ sudo ufw status

狀態： 啓用

至                          動作          來自
-                          --          --
1234                       ALLOW       Anywhere                  
1234 (v6)                  ALLOW       Anywhere (v6)  
```

增加iptable

```
$ sudo iptables -I INPUT -p tcp -m tcp --dport 1234 -j ACCEP
$ sudo iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     6    --  0.0.0.0/0            0.0.0.0/0            tcp dpt:1234

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination     
```

編譯執行以下的程式碼
```
$ vi server_test.cpp
$ g++ -o server_test server_test.cpp
$ ./server_test 1234
```

打開另一個ssh終端機，要確認有0 0.0.0.0:1234
```
$ sudo netstat -tunlp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:1234            0.0.0.0:*               LISTEN      33757/./server_test 
```

打開另一個ssh終端機，把之前的client的程式碼編譯執行
```
$ vi client_test.cpp
$ g++ -o client_test client_test.cpp
$ ./client_test 192.168.235.128 1234
```

執行結果

sever_test
```
$ ./server_test 1234
client已連上
收到client data = data 1
send = ok
收到client data = data 2
send = ok
收到client data = data 3
send = ok
iret = 0
```

client_test
```
$ ./client_test 192.168.235.128 1234
傳送給伺服器 = data 1
伺服器的回應 = ok
傳送給伺服器 = data 2
伺服器的回應 = ok
傳送給伺服器 = data 3
伺服器的回應 = ok
```

### server程式碼

建立監聽listen socket()函式 -> 轉換port變成big-endian -> bind()綁定port -> listen()監聽client個數 -> accept()接受client連進來

{% highlight c++ linenos %}
#include <iostream>
#include <cstring>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;
int main(int argc, char *argv[]) {
  if (argc != 2) {
    cout << "請輸入 ./server_test port" << endl;
    return -1;
  }
  // 建立監聽fd listen_fd，失敗傳回-1
  int listen_fd = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
  if (listen_fd == -1) {
    perror("socket");
    return -1;
  }
  // sockaddr_in結構
  struct sockaddr_in server_addr;
  memset(&server_addr, 0, sizeof(server_addr));
  server_addr.sin_family = AF_INET;
  // server port
  server_addr.sin_port = htons(atoi(argv[1]));
  // server有多個ip，所有ip都會監聽port
  server_addr.sin_addr.s_addr = htonl(INADDR_ANY);
  // 綁定監聽器，失敗傳回-1
  if (bind(listen_fd, (struct sockaddr*)&server_addr, sizeof(server_addr)) == -1) {
    perror("bind error");
    // 關閉監聽器
    close(listen_fd);
    return -1;
  }
  // 最多只讓3個client同時連server
  if (listen(listen_fd, 3)) {
    perror("listen");
    close(listen_fd);
    return -1;
  }
  // 接受client連接
  int client_fd = accept(listen_fd, 0, 0);
  if (client_fd == -1) {
    perror("accept");
    close(listen_fd);
    return -1;
  }
  cout << "client已連上" << endl;
  char buffer[1024];
  // 一定要用無限循環，等待client連上來
  while (true) {
    int iret;
    memset(buffer, 0, sizeof(buffer));
    // 如果client沒有send data，recv()會等待
    // 如果client斷線，recv()傳回0
    // 大於0代表有收到資料
    if ((iret = recv(client_fd, buffer, sizeof(buffer), 0)) <= 0) {
      cout << "iret = " << iret << endl;
      // 斷線 = 0就跳出無限循環
      break;
    }
    cout << "收到client data = " << buffer << endl;
    // 建立回應的資料給client
    strcpy(buffer, "ok");
    // 傳送回應的資料
    if ((iret = send(client_fd, buffer,strlen(buffer), 0)) <= 0) {
      perror("send");
      break;
    }
    cout << "send = " << buffer << endl;
  }
  close(listen_fd);
  close(client_fd);
  return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/file/file_desc.md %}#fd
[2]: {% link _pages/c/libc/errno.md %}
[3]: {% link _pages/c/basic/main.md %}
[4]: {% link _pages/c/basic/include.md %}