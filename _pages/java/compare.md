---
title: Comparable與Comparator
date: 2025-05-26
keywords: Java, Comparable, Comparator
---
Prerequisites:

- [介面][1]
- [泛型][2]
- [泛型介面][3]

## Comparable比較
Comparable是一個介面，有一個compareTo()方法，功用是比較大小。

compareTo()傳回值如下:
- 0 相等
- >0 大於
- <0 小於

Comparable介面原始檔
{% highlight java linenos %}
public interface Comparable<T> {
    public int compareTo(T o);
}
{% endhighlight %}


Comparator比較器是獨立一個類別，通常是用來傳進方法中，提供類別去使用它的比較方法。


[1]: {% link _pages/java/interface.md %}
[2]: {% link _pages/java/generics.md %}
[3]: {% link _pages/java/generics_interface.md %}