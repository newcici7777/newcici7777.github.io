---
title: StringBuilder vs StringBuffer
date: 2025-06-04
keywords: java, String Buffer, String Builder, String
---
StringBuilder使用方式跟StringBuffer一模一樣。

以下列出不同的地方。

## 執行緒安全
- [synchronized][1]

### StringBuffer有synchronized
StringBuffer在append()方法前面有synchronized。
{% highlight java linenos %}
public synchronized StringBuffer append(String str) {
    toStringCache = null;
    super.append(str);
    return this;
}
{% endhighlight %}

### StringBuilder沒synchronized
StringBuilder在append()方法前面是沒有synchronized。
{% highlight java linenos %}
public StringBuilder append(String str) {
    super.append(str);
    return this;
}
{% endhighlight %}

## 效率
### String
- [String][2]

以下的程式碼，執行到第2行，會產生以下步驟:
1. 變數s與\+會呼叫StringBuffer的append()方法。
2. 呼叫toString()方法。
3. new String()建立物件。
4. 在Heap中建立記憶體空間。
5. s變數指向Heap的記憶體位址。

String是final char[]陣列，經過步驟5，原本的`final char[] value = "a"`，這個物件就會變成孤兒，等待記憶體回收。

如果以下的程式碼進行2000次就會產生2000個孤兒，所以String的效率最慢。
{% highlight java linenos %}
String s = "a";
s += "b";
{% endhighlight %}

### StringBuffer與StringBuilder
StringBuffer與StringBuild不是final。

append、replace、delete，也只是對字元陣列進行修改，不會一直再建立新的物件，所以執行效率比String快。
{% highlight java linenos %}
char[] value = "a"
{% endhighlight %}

由於StringBuffer的append()是synchronized，一次只能被一個thread執行緒執行，其它執行緒們threads只能被卡住Blocked，等待synchronized方法中的執行緒做完，他們之中只有一個才能進入append()方法。

所以效率比Builder差，因為Builder不是執行緒安全。

## 執行效率程式碼
1秒 = 1000毫秒<br>
0.5秒 = 500毫秒<br>
0.1秒 = 100毫秒<br> 
0.001秒 = 1毫秒<br>
請自行換算，以下執行結果是以毫秒為單位。
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    long startT = System.currentTimeMillis();
    String str = "a";
    for (int i = 0; i < 20000; i++) {
      str += "b";
    }
    long endT = System.currentTimeMillis();
    System.out.println("String = " + (endT - startT));

    startT = System.currentTimeMillis();
    StringBuffer sb = new StringBuffer();
    for (int i = 0; i < 2000; i++) {
      sb.append(i);
    }
    endT = System.currentTimeMillis();
    System.out.println("StringBuffer = " + (endT - startT));

    startT = System.currentTimeMillis();
    StringBuilder sbder = new StringBuilder();
    for (int i = 0; i < 2000; i++) {
      sbder.append(i);
    }
    endT = System.currentTimeMillis();
    System.out.println("StringBuilder = " + (endT - startT));
  }
}
{% endhighlight %}
```
String = 92
StringBuffer = 2
StringBuilder = 1
```

由執行結果可以發現String要執行92毫秒，速度最慢。


[1]: {% link _pages/java/sync.md %}
[2]: {% link _pages/java/string.md %}#二個字串變數相加