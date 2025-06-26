---
title: String compareTo原始碼
day: 2025-05-26
keywords: Java, String compareTo
---
## value\[\]
String字串是用value\[\]來儲存字串。
{% highlight java linenos %}
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char value[];
}
{% endhighlight %}

## compareTo()
比較步驟如下:
```
1.取出2字串長度
     ↓
2.取得2個字串最小長度
     ↓
3.先比較最小長度內，每個字元是否相等
     ↓
4.字元不相等就傳回字元相減的結果
     ↓
5.若最小長度內的字元都相等
     ↓
6.傳回長度相減。
```
{% highlight java linenos %}
public int compareTo(String anotherString) {
    // 1.取出2字串長度
    int len1 = value.length;
    int len2 = anotherString.value.length;
    
    // 2.取得2個字串最小長度
    int lim = Math.min(len1, len2);

    char v1[] = value;
    char v2[] = anotherString.value;

    // 3.先比較最小長度內，每個字元是否相等
    int k = 0;
    while (k < lim) {
        char c1 = v1[k];
        char c2 = v2[k];
        // 4.字元不相等就傳回字元相減的結果
        if (c1 != c2) {
            return c1 - c2;
        }
        k++;
    }
    // 5.若最小長度內的字元都相等
    // 6.傳回長度相減
    return len1 - len2;
}
{% endhighlight %}

### 情況1
4.字元不相等就傳回字元相減的結果
```
value = abcdefg
another = abdd
```
結果為c(99) - d(100) = -1

程式碼
{% highlight java linenos %}
String value = "abcdefg";
String another = "abdd";
int result = value.compareTo(another);
{% endhighlight %}
-1

### 情況2
以下二個字串前4個字元都一樣，只有長度不同。<br>
5.若最小長度內的字元都相等<br>
6.傳回長度相減<br>
```
value = abcdefg
another = abcd
```
傳回3

程式碼
{% highlight java linenos %}
String value = "abcdefg";
String another = "abcd";
int result = value.compareTo(another);
System.out.println(result);
{% endhighlight %}
3

### 情況3
以下二個字串前4個字元都一樣，只有長度不同。<br>
5.若最小長度內的字元都相等<br>
6.傳回長度相減<br>
```
value = abcd
another = abcdfg
```
傳回-2

{% highlight java linenos %}
String value = "abcd";
String another = "abcdfg";
int result = value.compareTo(another);
System.out.println(result);
{% endhighlight %}
-2
