---
title: IO裝飾者模式
date: 2025-05-10
keywords: Java, Design patterns, Decorator Pattern, InputStream Pattern
---
Prerequisites:

- [裝飾者模式][1]
- [串流基礎][2]
- [檔案串流][3]

## 類別圖
以下類別圖與裝飾者模式幾乎一樣
![img]({{site.imgurl}}/pattern/decorator_io1.png)
InputStream_是抽象父類別，read()方法是子類別要實作覆寫內容。

FileInputStream_與ByteArrayInputStream_是InputStream_子類別，是最主要的類別，實作read()方法。

Decorator是裝飾者父類別，最重要的成員屬性是in，類型是InputStream_，由建構子Decorator(in)把in設值。

in成員屬性可以是主要類別FileInputStream_或ByteArrayInputStream_。

in成員屬性也可以是裝飾者類別，所以會用菱形實心線條指向自己，因為in成員屬性可以是自己。

Decorator.read()方法，是呼叫成員屬性in.read()

Decorator下面有二個子類別，都是裝飾者，不是主要的類別，分別是BufferInputStream_與ObjectInputStream_，都是為主要的類別增加「附加功能」。

FileInputStream_與ByteArrayInputStream_跟裝飾者模式中的Drink飲料(紅茶、綠茶、咖啡)一樣，是最主要的類別，其它BufferInputStream_與ObjectInputStream_都只是裝飾品，等同裝飾者模式中的Decorator類別(牛奶、珍珠、果凍)。

有些書本會說裝飾者模式是包裝者wrap模式，如下圖:

![img]({{site.imgurl}}/pattern/decorator_io2.png)

FileInputStream_是最主要類別，包在最裡面，BufferInputStream_與ObjectInputStream_都是裝飾者(包裝者)。

換成程式碼如下:
{% highlight java linenos %}
new ObjectInputStream_(new BufferInputStream_(new FileInputStream_()));
{% endhighlight %}
最裡面的是最主要的類別，因為它的建構子是不用有參數的。

![img]({{site.imgurl}}/pattern/decorator_io3.png)

ByteArrayInputStream_是最主要類別，包在最裡面，BufferInputStream_與ObjectInputStream_都是裝飾者(包裝者)。

換成程式碼如下:
{% highlight java linenos %}
new ObjectInputStream_(new BufferInputStream_(new ByteArrayInputStream_()));
{% endhighlight %}
最裡面的是最主要的類別，因為它的建構子是不用有參數的。

## 程式碼
### 抽象父類別InputStream_
{% highlight java linenos %}
public abstract class InputStream_ {
  public abstract String read();
}
{% endhighlight %}

### 主要類別，被裝飾(被包裝)的主類別
FileInputStream_
{% highlight java linenos %}
public class FileInputStream_ extends InputStream_{
  @Override
  public String read() {
    return "FileInputStream_";
  }
}
{% endhighlight %}

ByteArrayInputStream_
{% highlight java linenos %}
public class ByteArrayInputStream_ extends InputStream_{
  @Override
  public String read() {
    return "ByteArrayInputStream_";
  }
}
{% endhighlight %}

### 裝飾者Decorator
Decorator
{% highlight java linenos %}
public class Decorator extends InputStream_ {
  // 最重要的成員屬性
  private InputStream_ in;

  // 建構子
  public Decorator(InputStream_ in) {
    this.in = in;
  }

  @Override
  public String read() {
    // 呼叫成員屬性in.read()
    return in.read() ;
  }
}
{% endhighlight %}

BufferInputStream_
{% highlight java linenos %}
public class BufferInputStream_ extends Decorator{
  public BufferInputStream_(InputStream_ in) {
    super(in);
  }

  @Override
  public String read() {
    return super.read() + " + " + "This is BufferInputStream";
  }
}
{% endhighlight %}

ObjectInputStream_
{% highlight java linenos %}
public class ObjectInputStream_ extends Decorator{
  public ObjectInputStream_(InputStream_ in) {
    super(in);
  }

  @Override
  public String read() {
    return super.read() + " + " + "This is ObjectInputStream_";
  }
}
{% endhighlight %}

## Client測試程式
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) {
    // 來源串流是FileInputStream
    FileInputStream_ fileInputStream_ = new FileInputStream_();
    System.out.println(fileInputStream_.read());
    BufferInputStream_ bufferInputStream = new BufferInputStream_(fileInputStream_);
    System.out.println(bufferInputStream.read());
    ObjectInputStream_ objStream = new ObjectInputStream_(bufferInputStream);
    System.out.println(objStream.read());
    
    System.out.println("===============================");
    // 來源串流是FileInputStream
    ByteArrayInputStream_ byteArrstream = new ByteArrayInputStream_();
    System.out.println(byteArrstream.read());
    bufferInputStream = new BufferInputStream_(byteArrstream);
    System.out.println(bufferInputStream.read());
    objStream = new ObjectInputStream_(bufferInputStream);
    System.out.println(objStream.read());
  }
}
{% endhighlight %}
```
FileInputStream_
FileInputStream_ + This is BufferInputStream
FileInputStream_ + This is BufferInputStream + This is ObjectInputStream_
===============================
ByteArrayInputStream_
ByteArrayInputStream_ + This is BufferInputStream
ByteArrayInputStream_ + This is BufferInputStream + This is ObjectInputStream_
```

以上是此模式的精神是幫主要類別增加功能，真實的BufferInputStream提供readLine()的方法，讀取檔案時以一列一列的方式讀取，提高讀取效率，ObjectInputStream提供從檔案或記憶體byte[]陣列中讀取物件，但實際上讀取動作仍是由FileInputStream與ByteArrayInputStream在讀的，裝飾者是在做優化或額外附加功能。

[1]: {% link _pages/design_pattern/decorator.md %}
[2]: {% link _pages/java/io.md %}
[3]: {% link _pages/java/fileio.md %}