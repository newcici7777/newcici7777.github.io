---
title: List
date: 2025-06-27
keywords: java, List
---
List是可以放重覆的資料，新增的資料是有順序。

每一筆資料都是有索引。

## List方法
### get
參數為索引，取得某個位置的物件。
{% highlight java linenos %}
E get(int index);
{% endhighlight %}

程式碼
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
System.out.println(list.get(0));
{% endhighlight %}
```
10
```

### addAll
語法
```
boolean addAll(int index, Collection<? extends E> c);
```
在index索引位置，插入新的集合。
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
List list2 = new ArrayList();
list2.add("test");
list.addAll(1, list2);
System.out.println(list);
{% endhighlight %}
```
[10, test, true, str]
```

### indexOf
尋找物件所在位置。
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
int find = list.indexOf("str");
System.out.println(find);
{% endhighlight %}
```
2
```

### lastIndexOf
找到物件最後的索引位置
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
list.add("str");
list.add("str");
int find = list.lastIndexOf("str");
System.out.println(find);
{% endhighlight %}
```
4
```

### remove
移除某個索引的物件。
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
list.add("str");
list.add("str");
list.remove(4);
System.out.println(list);
{% endhighlight %}
```
[10, true, str, str]
```

### set修改
索引一定要小於集合的大小，不然會有例外。
{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
list.add("str");
list.add("str");
list.set(4, "Tom");
System.out.println(list);
{% endhighlight %}
```
[10, true, str, str, Tom]
```

### subList
取得子集合，從fromIndex到(toIndex - 1)，注意！不包含toIndex。
{% highlight java linenos %}
List<E> subList(int fromIndex, int toIndex);
{% endhighlight %}

{% highlight java linenos %}
List list = new ArrayList();
list.add(10);
list.add(true);
list.add("str");
list.add("str");
list.add("str");
List sub = list.subList(0, 2)
System.out.println(sub);
{% endhighlight %}
```
[10, true]
```

## ArrayList
- [synchronized][1]

1. 實際上程式碼是陣列。
2. 不是執行緒安全。

以下是ArrayList.add()，傳回值前面「沒有synchronized」。
{% highlight java linenos %}
    public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }
{% endhighlight %}

以下是Vector.add()，傳回值前面「有synchronized」。
{% highlight java linenos %}
    public synchronized boolean add(E e) {
        modCount++;
        add(e, elementData, elementCount);
        return true;
    }
{% endhighlight %}


[1]: {% link _pages/java/sync.md %}
