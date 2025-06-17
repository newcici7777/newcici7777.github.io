---
title: InputStreamReader與OutputStreamWriter
date: 2025-05-08
keywords: Java, 
---
Prerequisites:

- [Buffered串流][1]

## 轉換串流
InputStream與OutputStream是處理位元組資料的串流。

Reader與Writer抽象父類別，是處理字元資料的串流。

InputStream \+ Reader 位元組串流，可以變成讀取字元資料串流。

OutputStream \+ Writer 位元組串流，可以變成寫入字元資料串流。

InputStreamReader與OutputStreamWriter，建構子參數是InputStream與OutputStream。

意思是可以透過InputStreamReader與OutputStreamWriter，把「位元組」串流轉換成「字元」串流，還可以設定編碼。

可以處理亂碼的串流。

## InputStreamReader建構子
第2個參數設定編碼，若沒設定第2個參數，預設是用java vm 設定的編碼。
```
InputStreamReader(InputStream in)
InputStreamReader(InputStream in, String charsetName)
```

## OutputStreamWriter建構子
第2個參數設定編碼，若沒設定第2個參數，預設是用java vm 設定的編碼。
```
OutputStreamWriter(OutputStream out)
OutputStreamWriter(OutputStream out, Charset cs)
```

## InputStreamReader與FileInputStream
我有一個編碼為big5的文件，我嘗試讀取它。

Mac可以使用[文字編輯textedit](https://support.apple.com/zh-tw/guide/textedit/txted1028/mac)，設定編碼。

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws IOException {
    BufferedReader reader =
        new BufferedReader(new InputStreamReader(new FileInputStream("/Users/cici/testc/401"), "big5"));
    String line = reader.readLine();
    System.out.println(line);
    reader.close();
  }
}
{% endhighlight %}
```
你好台灣
```
## OutputStreamWriter與FileOutputStream
{% highlight java linenos %}
OutputStreamWriter writer = new OutputStreamWriter(new FileOutputStream("/Users/cici/testc/401"), "big5");
writer.write("測試測試");
writer.close();
{% endhighlight %}

## InputStreamReader與ByteArray串流
- [ByteArray串流][2]

除了檔案編碼，在Socket串流中，常常使用記憶體緩衝區串流(ByteArrayInputStream)與轉換串流的應用。
{% highlight java linenos %}
public class Test4 {
  public static void main(String[] args) throws IOException {
    ByteArrayInputStream byteArrInput = new ByteArrayInputStream("測試測試".getBytes());
    BufferedReader reader =
        new BufferedReader(new InputStreamReader(byteArrInput));
    String line = reader.readLine();
    System.out.println(line);
    reader.close();
  }
}
{% endhighlight %}
```
測試測試
```

[1]: {% link _pages/java/buffer_io.md %}
[2]: {% link _pages/java/bytearr_io.md %}#ByteArrayInputStream
