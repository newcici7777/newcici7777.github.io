---
title: Collection與Iterator
date: 2025-06-27
keywords: java, Collection, Iterator, hasNext(), next()
---
Prerequisites:

- [Wrapper Classes][1]

集合介面Interface的繼承圖如下:

![img]({{site.imgurl}}/java/.png)

上圖每一個都是介面Interface。

Collection介面下面有List介面與Set介面，List是可以放重覆的資料，Set不可以放重覆的資料。

List是有順序的，Set是沒有順序。

Iterator介面是集合的疊代器，用來遍歷集合。

## Collection
提供以下方法，只要是實作Collection介面的子類別，都可以用以下方法。<br>

1. add 新增
2. remove 刪除
3. contains 是否在集合中？
4. size 大小
5. isEmpty
6. clear
7. addAll
8. containsAll
9. removeAll

### add
以下程式碼中的10與true，都是基本型態，加到List時，會自動轉成包裝類別存進List中。<br>
還可以把null加入。
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);    // 轉成Integer
list.add(true);  // 轉成Boolean
list.add(null);  // 可以把null加入
list.add("str"); // 物件加入
{% endhighlight %}

### remove
參數可以是要刪除的物件，也可以是要刪除的索引。
{% highlight java linenos %}
boolean remove(Object o);
E remove(int index);
{% endhighlight %}

{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
list.remove(true); // 刪除的物件
list.remove(0);    // 刪除的索引
{% endhighlight %}

### contains
集合中有沒有某個物件，有就傳回true，沒有就傳回false。
{% highlight java linenos %}
boolean contains(Object o);
{% endhighlight %}

### isEmpty
判斷是否為空。
{% highlight java linenos %}
boolean isEmpty();
{% endhighlight %}

### clear
刪除集合中所有資料。

### addAll
加入另一個集合。
{% highlight java linenos %}
boolean addAll(Collection c);
{% endhighlight %}

程式碼
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
List list2 = new ArrayList();
list2.add("test");
list.addAll(list2);
{% endhighlight %}

### containsAll
判斷「另一個」集合，有沒有在「目前這個」集合中，有就傳回true，沒有就傳回false。
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
List list2 = new ArrayList();
list2.add("test");
list.addAll(list2);
System.out.println(list.containsAll(list2));
{% endhighlight %}
```
true
```

## iterator遍歷集合
- [自己寫一個Iterator][1]

透過iterator()方法，可以取得Iterator疊代器物件。
{% highlight java linenos %}
Iterator<E> iterator();
{% endhighlight %}

Collection介面繼承Iterator，所有的繼承Collection介面的子類別，一定會有iterator()方法。

Iterator介面有hasNext()、next()、remove()三個方法。

hasNext(): 檢查有沒有元素(element)？傳回值型態為boolean

next(): 傳回元素(element)，並把指向陣列索引的變數，指向下一個索引。

{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
Iterator iter = list.iterator();
while (iter.hasNext()) {
    Object obj = iter.next();
    System.out.println(obj.toString());
}
{% endhighlight %}

若要再重新使用疊代器，再重新取得一次iterator()。
{% highlight java linenos %}
iter = list.iterator();
{% endhighlight %}

{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
Iterator iter = list.iterator();
while (iter.hasNext()) {
  Object obj = iter.next();
  System.out.println(obj.toString());
}
iter = list.iterator();
while (iter.hasNext()) {
  Object obj = iter.next();
  System.out.println(obj.toString());
}
{% endhighlight %}
```
10
true
str
10
true
str
```

## 增強式for迴圈
實際上原始碼也是使用Iterator，但使用for讓使用者更方便。<br>
把集合中每一個元素取出來，放在變數中。<br>
語法
```
for (元素的類別 變數 : 集合) {
  // 要執行的程式碼
}
```

{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
for (Object o : list) {
  System.out.println(o.toString());
}
{% endhighlight %}

### 增強式for迴圈的原始碼
- [Debug][2]
可以在for 前面加上中斷點，Debug執行。<br>
1.step into，就會進到JDK原始碼。
{% highlight java linenos %}
for (Object o : list) {
}
{% endhighlight %}

原始碼中，會進到以下程式碼，建立Itr()，證明增強式for迴圈，實際上是使用Iterator。
{% highlight java linenos %}
    public Iterator<E> iterator() {
        return new Itr();
    }
{% endhighlight %}

2.step Out離開iterator()方法。<br>
3.setp Into<br>
4.接著進入hasNext()方法<br>
{% highlight java linenos %}
public boolean hasNext() {
    return cursor != size;
}
{% endhighlight %}

5.step Out離開hasNext()方法。<br>
6.setp Into<br>
7.接著進入next()方法
{% highlight java linenos %}
public E next() {
    checkForComodification();
    int i = cursor;
    if (i >= size)
        throw new NoSuchElementException();
    Object[] elementData = ArrayList.this.elementData;
    if (i >= elementData.length)
        throw new ConcurrentModificationException();
    cursor = i + 1;
    return (E) elementData[lastRet = i];
}
{% endhighlight %}

8.step Out離開next()方法。<br>
9.此時就會移動到`for (Object o : list) {`的下一行位置。<br>

### 增強式for迴圈的快速鍵
I \+ Enter




[1]: {% link _pages/design_pattern/iterator.md %}
[2]: {% link _pages/java/debug.md %}