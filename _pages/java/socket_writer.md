---
title: Socket與Writer
date: 2025-06-18
keywords: Java, Socket, Writer
---
Prerequisites:

- [串流基礎][1]
- [轉換串流][2]
- [Buffered串流][3]

在串流基礎提過，串流有二種，一種是位元組串流，一種是字元串流。

讀取寫入的位置有二種，一種是檔案，一種是記憶體緩衝區。

Socket存取與寫入的位置都是在記憶體緩衝區，而且是位元組串流。

要把Socket位元組串流轉成字元串流，需要使用轉換串流(OutputStreamWriter,InputStreamReader)。

```mermaid
flowchart LR
    位元組串流 -- 轉 --> 轉換串流 -- 轉 --> 字元串流
```

因為Socket存取與寫入的位置都在記憶體，所以需要使用Buffered字元串流(BufferedWriter,BufferedReader)。

## 重要的方法介紹
### flush()
字元串流要使用flush()方法，把記憶體緩衝區的資料送到socket.OutputStream。

## Client傳送文字
傳送步驟如下:
1. 取得socket的OutputStream。
2. 透過轉換串流取得BufferedWriter。
3. 寫入文字。
4. flush，通知伺服器已傳送完成。
5. 關閉串流。

{% highlight java linenos %}
public class ClientWriter {
  public static void main(String[] args) throws IOException {
    Socket socket = new Socket("127.0.0.1", 9999);
    OutputStream os = socket.getOutputStream();
    // OutputStreamWriter是轉換串流，把位元組串流轉成字元串流
    // BufferedWriter 寫入的位置都在記憶體緩衝區
    BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(os));
    writer.write("哈囉。 Server");
    writer.newLine();
    writer.write("hi");
    // 告訴Server傳輸完畢
    writer.flush();
    // 關閉串流
    writer.close();
    os.close();
    socket.close();
  }
}
{% endhighlight %}

## Server接收文字
{% highlight java linenos %}
public class ServerReader {
  public static void main(String[] args) throws IOException {
    // 設定監聽的port number
    ServerSocket serverSocket = new ServerSocket(9999);
    // accept會一直等待有人連上，有人連上會傳回socket物件
    Socket socket = serverSocket.accept();
    // socket包含客戶端傳送的資料，用InputStream取得
    InputStream is = socket.getInputStream();
    // InputStreamReader是轉換串流，把位元組串流轉成字元串流
    // BufferedReader 讀取的位置都在記憶體緩衝區
    BufferedReader reader = new BufferedReader(new InputStreamReader(is));
    String line = null;
    while((line = reader.readLine()) != null) {
      System.out.println(line);
    }
    reader.close();
    is.close();
    socket.close();
    serverSocket.close();
  }
}
{% endhighlight %}

## 執行程式
1. 先執行Server
2. 再執行Client

![img]({{site.imgurl}}/java/socket1.png)

[1]: {% link _pages/java/io.md %}
[2]: {% link _pages/java/convert_io.md %}
[3]: {% link _pages/java/buffer_io.md %}