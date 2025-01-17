---
title: 多工的socket
date: 2025-01-12
keywords: c++, Socket, fork
---

之前的socket是一對一連線，也就是一個socket對一個client。

此篇是一個Server，可以讓多個Client連線。

每次接受Client連線後，就fork一個子程序去做傳送資料與接收資料的事。

## 父程序流程

監聽listen socket()函式 -> 轉換port變成big-endian -> bind()綁定ip與port -> listen()開始監聽 -> accept()接受client連線，傳回client socket

對於父程序來說，只要管理listen socket，關閉listen socket。

## 子程序流程

send()傳送資料 -> recv()接收資料

對於子程序而言，只要管理client socket，關閉client socket。

## 多工Server Socket程式碼

- TCPServer tcpServer;設為全域變數
- include signal.h
- 複製[kill][1]的FatherExit()與ChildExit()函式
- 加上while
- fork複製出子程序
- fork複製出子程序後，父程序就不需要管理client socket，關掉client socket
- 子程序不用管理監聽的socket，所以關掉它
- 子程序執行完，要關閉子程序，return 0;在無限迴圈內關閉子程序，while(true){... return 0;}

[1]: {% link _pages/c/libc/kill.md %}

<pre>
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
// 設成全域變數
// 建立監聽fd listen_fd
<span class="markline">
TCPServer tcpServer;
// 父程序離開
void FatherExit(int sig) {
  // 怱略二次收到相同訊號
  // 使用kill(0, 訊號)會發給子程序也會再次發給發送訊號的父程序自己
  signal(SIGINT, SIG_IGN);  // 怱略ctrl+c
  signal(SIGTERM, SIG_IGN);  // 怱略kill -15
  cout << "父程序退出， signal = " << sig << endl;
  kill(0, SIGTERM);  // 向全部的子程序發送15訊號，終止子程序
  // 父程序管理listen socket，父程序要離開要釋放記憶體
  // 關閉listen socket
  tcpServer.closeListen();
  exit(0);  // 父程序離開
}
// 子程序離開
void ChildExit(int sig) {
  // 防止再次收到相同訊號
  signal(SIGINT, SIG_IGN);  // 怱略ctrl+c
  signal(SIGTERM, SIG_IGN);  // 怱略kill -15
  cout << "子程序 pid = " << getpid() << " 退出， signal = " << sig << endl;
  // 子程序管理client socket，子程序要離開要釋放記憶體
  // 關閉client socket
  tcpServer.closeClient();
  exit(0);  // 子程序離開
}
</span>
int main(int argc, char *argv[]) {
  if (argc != 2) {
    cout << "請輸入 ./server_test port" << endl;
    return -1;
  }
  <span class="markline">
  // 略過全部訊號
  for (int i = 0; i <= 64; i++) signal(i, SIG_IGN);
  // 父程序要補捉的訊號 ctrl + c
  signal(SIGTERM, FatherExit);
  // 父程序要補捉kill -15
  signal(SIGINT, FatherExit);
  </span>
  // 初始化監聽
  if (tcpServer.initListen(atoi(argv[1])) == false) {
    perror("initListen");
    return -1;
  }
  <span class="markline">
  // 無限循環，一直為Client提供服務
  while (true) {</span>
    // 接受client連接
    if (tcpServer.accept() == false) {
      perror("accept");
      return -1;
    }
    <span class="markline">
    // fork後，會把client socket與listen socket複製一份給子程序
    int pid = fork();
    if (pid == -1) {
      perror("fork()");
      return -1;
    }
    // 大於0是父程序，直接跳到while()條件句，等待Client來連線
    if (pid > 0) {
      // fork複製出子程序後，父程序就不需要管理client socket，這個是子程序管理
      // 關掉client socket
      tcpServer.closeClient();
      continue;
    }
    // 以下是子程序要跑的程式碼
    // 子程序不用管理監聽的socket，所以關掉它
    tcpServer.closeListen();
    // 子程序要補捉的訊號 ctrl + c
    signal(SIGTERM, ChildExit);
    // 子程序要補捉kill -15
    signal(SIGINT, ChildExit);
    </span>
    cout << "client已連上 = " << tcpServer.getClientIp() << endl;
    string buffer;
    while (true) {
      // 如果client沒有send data，recv()會等待
      // 收到0，代表斷線
      if (tcpServer.recv(buffer, 1024) == false) {
        perror("recv()");
        break;
      }
      cout << "收到client data = " << buffer << endl;
      // 建立回應的資料給client
      buffer = "ok";
      // 傳送回應的資料
      if (tcpServer.send(buffer) == false) {
        perror("send");
        break;
      }
      cout << "send = " << buffer << endl;
    }
    <span class="markline">
    // 子程序處理用戶端傳輸，傳輸接收完畢後，要用return 0關閉子程序
    // 若不關閉又會回到accept()函式
    return 0;
    </span>
  }
}
</pre>


