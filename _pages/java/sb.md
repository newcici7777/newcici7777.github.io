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

## append
append傳回值就是StringBuffer。
{% highlight java linenos %}
    StringBuffer sb = new StringBuffer();
    StringBuffer sb2 = sb.append("abc");
{% endhighlight %}

會把基本型別，自動轉成字串。
{% highlight java linenos %}
    StringBuffer sb = new StringBuffer();
    sb.append(true).append(10.5).append(10);
    System.out.println(sb);
{% endhighlight %}
```
true10.510
```

把boolean轉成字串原始碼。
{% highlight java linenos %}
public AbstractStringBuilder append(boolean b) {
    ensureCapacityInternal(count + (b ? 4 : 5));
    int count = this.count;
    byte[] val = this.value;
    if (isLatin1()) {
        if (b) {
            val[count++] = 't';
            val[count++] = 'r';
            val[count++] = 'u';
            val[count++] = 'e';
        } else {
            val[count++] = 'f';
            val[count++] = 'a';
            val[count++] = 'l';
            val[count++] = 's';
            val[count++] = 'e';
        }
    }
{% endhighlight %}


## StringBuffer轉String
使用toString()或者使用String建構子放入StringBuffer。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    StringBuffer sb = new StringBuffer("Hello World");
    // 使用toString()
    String str1 = sb.toString();
    // 使用String建構子
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

## toString原始碼
重點是 new String()，new String是在heap建立記憶體空間，不是在String pool建立記憶體空間。
{% highlight java linenos %}
    public synchronized String toString() {
        if (toStringCache == null) {
            return toStringCache = new String(this, null);
        }
        // 重點是 new String
        return new String(toStringCache);
    }
{% endhighlight %}


## StringBuffer常用方法
### delete
```
StringBuffer delete(int start, int end)
start <= range < end
```
range就是要刪的範圍，不包含end。
{% highlight java linenos %}
 StringBuffer sb = new StringBuffer("Hello world,貓咪");
 // 1 <= x < 4 
 sb.delete(1,4);
 System.out.println(sb);
{% endhighlight %}
```
Ho world,貓咪
```

### replace
修改可當作replace取代。
```
StringBuffer replace(int start, int end, String str)
start <= range < end
```
range就是要取代的範圍，不包含end。
{% highlight java linenos %}
  StringBuffer sb = new StringBuffer("貓咪Hello world");
  sb.replace(1, 4, "狗");
  System.out.println(sb);
{% endhighlight %}
```
貓狗llo world
```

### indexOf
第一次找到的位置，由左往右。
{% highlight java linenos %}
  StringBuffer sb = new StringBuffer("貓咪Hello world");
  int index = sb.indexOf("咪");
  System.out.println(index);
{% endhighlight %}
```
1
```

### insert
在指定位置插入字串，原本的位置往後移到插入字串的後面。
{% highlight java linenos %}
  StringBuffer sb = new StringBuffer("貓咪Hello world");
  sb.insert(1, "恐龍");
  System.out.println(sb);
{% endhighlight %}
```
貓恐龍咪Hello world
```

### length長度
{% highlight java linenos %}
StringBuffer sb = new StringBuffer("貓咪Hello world");
int len = sb.length();
{% endhighlight %}

## null
### 建構子傳null
{% highlight java linenos %}
  String str = null;
  StringBuffer sb = new StringBuffer(str);
  System.out.println(sb.length());
{% endhighlight %}

在父類別AbstractStringBuilder的建構子中，str.length()會拋出例外，因為str是null。
{% highlight java linenos %}
AbstractStringBuilder(String str) {
    int length = str.length();
    // ....
}
{% endhighlight %}

### append傳null
先到父類別的appendNull()方法
{% highlight java linenos %}
public AbstractStringBuilder append(String str) {
    if (str == null) {
        return appendNull();
    }
    // ....
}
{% endhighlight %}

appendNull，會把null的字串加在字元陣列中。
{% highlight java linenos %}
private AbstractStringBuilder appendNull() {
    ensureCapacityInternal(count + 4);
    int count = this.count;
    byte[] val = this.value;
    if (isLatin1()) {
        val[count++] = 'n';
        val[count++] = 'u';
        val[count++] = 'l';
        val[count++] = 'l';
    } else {
    // ....
}
{% endhighlight %}

以下執行結果為4
{% highlight java linenos %}
  String str = null;
  StringBuffer sb = new StringBuffer();
  sb.append(str);
  System.out.println(sb.length());
{% endhighlight %}
4

## lastIndexOf與insert進階
lastIndexOf是由最後面往前找，找到就傳回位置。

12345.67 
轉成 
12,345.67

{% highlight java linenos %}
  StringBuffer sb = new StringBuffer("12345.67");
  // 找到小數點
  int index = sb.lastIndexOf(".");
  // 小數點前三位逗號
  sb.insert(index - 3, ",");
  System.out.println(sb);
{% endhighlight %}
```
12,345.67
```

那如果前面的數字很長呢？例如:12345678910111213.11

{% highlight java linenos %}
  StringBuffer sb = new StringBuffer("12345678910111213.11");
  // -3要放在初始值。
  int index = sb.lastIndexOf(".") - 3;
  for (int i = index; i > 0; i -= 3) {
    sb.insert(i, ",");
  }
  System.out.println(sb);
{% endhighlight %}
```
12,345,678,910,111,213.11
```

[1]: {% link _pages/java/string.md %}