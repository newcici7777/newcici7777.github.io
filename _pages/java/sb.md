---
title: String Buffer
date: 2025-06-04
keywords: java, String Buffer
---
Prerequisites:

- [String][1]

## 字串存放位置
String Buffer繼承AbstractStringBuilder。

字串存放位置是在AbstractStringBuilder中的value變數，不是final，char\[\]陣列是物件，是存在Heap中。
{% highlight java linenos %}
    char[] value;
    AbstractStringBuilder(int capacity) {
        value = new char[capacity];
    }
{% endhighlight %}

### String Buffer vs String
String存放字串的位置，是final的，所以更新字串，就是建立新的物件，因為final不能被修改內容。

String Buffer存放字串的位置，不是final，所以更新字串，是更新物件內容，因為不用重新建立物件，所以效率比較快。

## 建構子
StringBuffer的空建構子，預設建立char\[16\]的大小。
{% highlight java linenos %}
  public StringBuffer() {
      super(16);
  }
{% endhighlight %}

可自訂char\[\]大小。
{% highlight java linenos %}
StringBuffer stringBuffer = new StringBuffer(100);
{% endhighlight %}

使用字串建立StringBuffer。
{% highlight java linenos %}
StringBuffer stringBuffer = new StringBuffer("Hello World");
{% endhighlight %}

建立char\[字串長度 + 16\]的大小。
{% highlight java linenos %}
public StringBuffer(String str) {
    super(str.length() + 16);
    append(str);
 }
{% endhighlight %}

## StringBuffer轉String
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    StringBuffer sb = new StringBuffer("Hello World");
    String str1 = sb.toString();
    String str2 = new String(sb);
    System.out.println(str1);
    System.out.println(str2);
  }
}
{% endhighlight %}
```
Hello World
Hello World
```

## StringBuffer常用方法



[1]: {% link _pages/java/string.md %}