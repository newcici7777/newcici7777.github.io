---
title: ArrayList原始碼
date: 2025-06-27
keywords: java, List
---
## 空建構子
如果使用空的建構子，預設一開始的陣列大小是10。
{% highlight java linenos %}
List list = new ArrayList();
{% endhighlight %}

若之後增加超過10，會以1.5倍的方式增加容量。

### Debug設定
要Debug ArrayList查看容量變化大小，要把以下勾勾取消。

![img]({{site.imgurl}}/editor/debug12.png)

### Debug ArrayList
- [Debug][1]

#### 空建構子
1.斷點設在下面程式碼第一行。<br>
2.執行Debug<br>
3.step into<br>
{% highlight java linenos %}
List list = new ArrayList();
for (int i = 0; i < 10; i++) {
  list.add(i);
}
{% endhighlight %}

4.進入無參數建構子<br>
5.step Out<br>
{% highlight java linenos %}
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
{% endhighlight %}

6.此時游標會停留在第一行，step over<br>
7.此時游標會停留在第二行，step over<br>
8.此時游標會停留在第三行，step into<br>
{% highlight java linenos %}
List list = new ArrayList();
for (int i = 0; i < 10; i++) {
  list.add(i);
}
{% endhighlight %}

9.此時進入包裝類別的valueOf()方法，step out離開此方法。<br>
10.此時游標會停留在第三行，沒有在下一行，代表還有方法沒執行完，step into方法。<br>
{% highlight java linenos %}
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
{% endhighlight %}

11.進到add方法，step over到下一行。
12.step into進到`add(e, elementData, size);`
{% highlight java linenos %}
public boolean add(E e) {
    modCount++;  // 集合被修改的次數，防止多執行緒同時修改
    add(e, elementData, size);
    return true;
}
{% endhighlight %}

13.s為0，elementData.length也為0，step into grow()方法。
{% highlight java linenos %}
private void add(E e, Object[] elementData, int s) {
    if (s == elementData.length)
        elementData = grow();
    elementData[s] = e;
    size = s + 1;
}
{% endhighlight %}

14. oldCapacity為0，minCapacity為0，DEFAULT_CAPACITY為10，
執行`new Object[10]`
{% highlight java linenos %}
private Object[] grow(int minCapacity) {
    int oldCapacity = elementData.length;
    if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        int newCapacity = ArraysSupport.newLength(oldCapacity,
                minCapacity - oldCapacity, // minimum growth 
                oldCapacity >> 1           // preferred growth );
        return elementData = Arrays.copyOf(elementData, newCapacity);
    } else {
        return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
    }
}
{% endhighlight %}

15.一路step out離開方法。

#### 第11個資料
斷點設在`list.add(11);`。
{% highlight java linenos %}
    List list = new ArrayList();
    for (int i = 0; i < 10; i++) {
      list.add(i);
    }
    list.add(11);
    System.out.println(list);
{% endhighlight %}

一路step into到下面這個方法
{% highlight java linenos %}
    public static int newLength(int oldLength, int minGrowth, int prefGrowth) {
        // preconditions not checked because of inlining
        // assert oldLength >= 0
        // assert minGrowth > 0

        int prefLength = oldLength + Math.max(minGrowth, prefGrowth); // might overflow
        if (0 < prefLength && prefLength <= SOFT_MAX_ARRAY_LENGTH) {
            return prefLength;
        } else {
            // put code cold in a separate method
            return hugeLength(oldLength, minGrowth);
        }
    }
{% endhighlight %}

重點是以下這二行，oldCapacity為10，prefLength為10 \+ 5
{% highlight java linenos %}
oldCapacity >> 1 // 10除2 = 5
int prefLength = oldLength + Math.max(minGrowth, prefGrowth);
{% endhighlight %}

![img]({{site.imgurl}}/editor/debug13.png)

確實集合的大小變成15，上圖11-14是null。

[1]: {% link _pages/java/debug.md %}