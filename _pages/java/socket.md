---
title: Socket
date: 2025-06-17
keywords: Java, socket
---
## Socket
Socket是用戶端(Client)與伺服器Server之間的連線。

二台主機之間的連線透過IO串流傳輸。

## Client傳送資料到Server
### 建立Server Socket
伺服器端操作:
1. 設定埠port number。
2. accept()監聽有沒有用戶連上port。
3. 用戶連上，取得socket。
4. socket.getInputStream()讀取傳來的資料。
5. 關閉串流

{% highlight java linenos %}
public class Server {
  public static void main(String[] args) throws IOException {
    // 1.設定監聽的port number
    ServerSocket serverSocket = new ServerSocket(9999);
    // 2.accept會一直等待有人連上
    // 3.有人連上會傳回socket物件
    Socket socket = serverSocket.accept();
    // 4.socket包含客戶端傳送的資料
    InputStream is = socket.getInputStream();
    // 讀取資料
    byte[] buff = new byte[1024];
    int len = 0;
    while((len = is.read(buff)) != -1) {
      System.out.println(new String(buff, 0, len));
    }
    // 關閉串流
    is.close();
    socket.close();
    serverSocket.close();    
  }
}
{% endhighlight %}

### 建立用戶端Socket
傳送資料步驟如下:
1. 連接伺服器ip與埠port。
2. 連接成功取得socket物件。
3. 取得輸出串流socket.getOutputStream()
4. 寫入資料至輸出串流。
5. shutdownOutput() 通知Server接收資料。
6. 關閉串流。
7. 關閉socket。

{% highlight java linenos %}
public class Client {
  public static void main(String[] args) throws IOException {
    // 1. 連接伺服器ip與埠port。
    // 2. 連接成功取得socket物件。
    Socket socket = new Socket("127.0.0.1", 9999);
    // 3. 取得輸出串流socket.getOutputStream()
    OutputStream os = socket.getOutputStream();
    // 4. 寫入資料至輸出串流。
    os.write("Hello, socket".getBytes());
    // 5. shutdownOutput() 通知Server接收資料
    socket.shutdownOutput();
    // 6. 關閉串流。
    os.close();
    // 7. 關閉socket
    socket.close();
  }
}
{% endhighlight %}
```
Hello, socket
```

## Server傳送資料到Client
### Server
{% highlight java linenos %}
public class Server {
  public static void main(String[] args) throws IOException {
    ServerSocket serverSocket = new ServerSocket(9999);
    Socket socket = serverSocket.accept();
    InputStream is = socket.getInputStream();
    byte[] buff = new byte[1024];
    int len = 0;
    while((len = is.read(buff)) != -1) {
      System.out.println(new String(buff, 0, len));
    }

    // 傳送資料，取得OutputStream
    OutputStream os = socket.getOutputStream();
    // 寫入資料
    os.write("收到資料".getBytes());
    // 通知Client接收資料
    socket.shutdownOutput();
    // 關閉串流
    os.close();
    is.close();
    socket.close();
    serverSocket.close();
  }
}
{% endhighlight %}
```
Hello, socket
```
### Client
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) throws IOException {
    Socket socket = new Socket("127.0.0.1", 9999);
    OutputStream os = socket.getOutputStream();
    os.write("Hello, socket".getBytes());
    socket.shutdownOutput();
    InputStream is = socket.getInputStream();
    // 讀取資料
    byte[] buff = new byte[1024];
    int len = 0;
    while((len = is.read(buff)) != -1) {
      System.out.println(new String(buff, 0, len));
    }
    os.close();
    socket.close();
  }
}
{% endhighlight %}
```
收到資料
```