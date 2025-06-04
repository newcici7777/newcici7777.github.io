---
title: String
date: 2025-05-26
keywords: Java, String
---
Prerequisites:

- [Memory Model][4]
- [== 比較][2]

## 建立方式
建立方式有二種，一種是指向字串常數，一種是使用建構子。

### 字串常數
什麼是字串常數？\"\"雙引號包住的字串是常數。

{% highlight java linenos %}
String s1 = "這個就是字串常數";
{% endhighlight %}

字串常數記憶體模型

![img]({{site.imgurl}}/java/str1.png)

1. 在String pool找有沒有Hello的字串，沒有就建立記憶體空間放Hello。
2. Stack中s1變數指向String pool中Hello的記憶體位址。

### 建構子
{% highlight java linenos %}
String s1 = new String(String s);
{% endhighlight %}


#### String有一個value屬性
value是final char\[\]，存放字串。

{% highlight java linenos %}
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    // 存放字串
    private final char value[];
}
{% endhighlight %}

#### 建構子記憶體模型

![img]({{site.imgurl}}/java/str2.png)

1. 建立記憶體空間
2. 將Stack中s1變數指向記憶體位址
3. 在String pool找有沒有Hi的字串，沒有就建立記憶體空間放Hi。
4. String有一個value屬性，指向String pool中的Hi記憶體位址。

### 建構子與常數的記憶體位址不同
從以上的記憶體模型，可以發現常數建立的字串，是指向String Pool中的記憶體位址。

而用new String()建立的字串，變數指向的是Heap中的記憶體位址。

String是類別，==對類別而言，是用來比較記憶體位址是否相等。

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    String s1 = "Hello";
    String s2 = "Hello";
    System.out.println("s1 == s2 " + (s1 == s2));
    String s3 = new String("Hello");
    String s4 = new String("Hello");
    System.out.println("s3 == s4 " + (s3 == s4));
    System.out.println("s1 == s3 " + (s1 == s3));
  }
}
{% endhighlight %}
```
s1 == s2 true
s3 == s4 false
s1 == s3 false
```

### 二者建立方式的記憶體模型
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    String s1 = "Hello";
    String s2 = new String("Hello");
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/java/str3.png)

1. 在String pool找有沒有Hello的字串，沒有就建立記憶體空間放Hello。
2. s1變數指向String Pool中Hello的記憶體位址。

![img]({{site.imgurl}}/java/str4.png)

1. 建立記憶體空間
2. 將Stack中s2變數指向記憶體位址
3. 在String pool找有沒有Hello的字串，沒有就建立記憶體空間放Hello。
4. String有一個value屬性，指向String pool中的Hello記憶體位址。

## String equals

- [String與equals()][2]

相同的內容不想再寫一遍，請詳見上述連結。

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    String s1 = "Hello";
    String s2 = new String("Hello");
    System.out.println("s1 equals s2 " + s1.equals(s2));
    System.out.println("s1 == s2 " + (s1 == s2));
  }
}
{% endhighlight %}
```
s1 equals s2 true
s1 == s2 false
```

## String intern()
intern() 取出String.value指向String Pool中的記憶體位址。

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    String s1 = "Hello";
    String s2 = new String("Hello");
    System.out.println("s1 equals s2 " + s1.equals(s2));
    System.out.println("s1 == s2 " + (s1 == s2));
    System.out.println("s1 == s2.intern() " + (s1 == s2.intern()));
    System.out.println("s2 == s2.intern() " + (s2 == s2.intern()));
    // s2 指向Heap中的記憶體位址
    // s2.intern()指向String Pool中的記憶體位址
  }
}
{% endhighlight %}
```
s1 equals s2 true
s1 == s2 false
s1 == s2.intern() true
s2 == s2.intern() false
```
## String被指派其它字串就是建立物件
Prerequisites:

- [final][5]

value是final char\[\]，存放字串。

因為value屬性是final，不能直接指向其它陣列，所以String物件被指派其它字串，實際上就是在String Pool中建立新的String物件，並非更新字元陣列裡面的內容。
{% highlight java linenos %}
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    // 存放字串
    private final char value[];
}
{% endhighlight %}

String Pool建立Hello，s1指向Hello記憶體位址。

![img]({{site.imgurl}}/java/str5.png)

String Pool建立Hi，s1指向Hi記憶體位址。

![img]({{site.imgurl}}/java/str6.png)

Hello在String Pool中有，s1指向Hello記憶體位址。

![img]({{site.imgurl}}/java/str7.png)

## 二個字串常數相加
編譯器看到`"Hello" + " World"`就視為一組字串。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    String s1 = "Hello" + " World";
    String s2 = "Hello World";
    System.out.println("s1 == s2 " + (s1 == s2));
  }
}
{% endhighlight %}
```
s1 == s2 true
```

由結果可知s1與s2是相等的。

## 二個字串變數相加
字串變數相加會呼叫StringBuilder的append()方法，最後會回傳`new String(value, 0, cout)`

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    String s1 = "Hello";
    String s2 = " World";
    String s3 = s1 + s2;  // 傳回Heap中的記憶體位址
    String s4 = "Hello World";  // 傳回String Pool中的記憶體位址
    System.out.println("s3 == s4 " + (s3 == s4));
  }
}
{% endhighlight %}
```
s3 == s4 false
```

把上面`s3 = s1 + s2;`的程式碼過程如下。

{% highlight java linenos %}
  StringBuilder sb = new StringBuilder();
  sb.append("Hello");
  sb.append(" World");
  String s3 = sb.toString();
{% endhighlight %}

## String實作Comparable

- [String與Comparable][1]

請詳見上述連結。

## String 與 IO

- [String與`char[]`和`byte[]`][3]

請詳見上述連結。

[1]: {% link _pages/java/compare.md %}
[2]: {% link _pages/java/equals_compare.md %}
[3]: {% link _pages/java/string.md %}
[4]: {% link _pages/java/memory_model.md %}
[5]: {% link _pages/java/final.md %}