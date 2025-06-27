---
title: Iterator
date: 2025-04-29
keywords: Java, Design patterns, Iterator Pattern
---
以下的類別圖分二類，左邊是集合Aggregate，右邊是Iterator疊代器。

![img]({{site.imgurl}}/pattern/iter2.png)

## Aggregate介面
### createIterator
createIterator()取得Iterator，實作Aggregate介面的子類別都要實作此方法。

## 實作Aggregate介面的子類別
### elements
成員屬性有elements，可以是陣列、List、任何集合類別。

### createIterator
子類別要自己覆寫此方法，取得Iterator。

elements是陣列，就要取得陣列的Iterator。<br>
elements是List，就要取得List的Iterator。<br>elements是Map，就要取得Map的Iterator。<br>

## Iterator
重點！Iterator使用Java原本就有的，不另外自己寫。

Iterator介面有hasNext()、next()、remove()三個方法要實作。

hasNext(): 有沒有元素(element)？傳回值型態為boolean

next(): 先傳回元素(element)，再把索引\+\+。

remove(): 刪除元素

實作Iterator類別，這個要自己實作。

## 陣列集合與Iterator
![img]({{site.imgurl}}/pattern/iter2.png)

寫一個電腦課程的集合與iterator

### Aggregate介面
{% highlight java linenos %}
public interface Aggregate {
  Iterator createIterator();
}
{% endhighlight %}

### 實作Aggregate介面的子類別
Course(課程)
{% highlight java linenos %}
public class Course {
  private String name;

  public Course(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }
}
{% endhighlight %}

電腦課程集合ComputerCourse，實作Aggregate介面。
1. 實作createIterator()方法
2. 把集合中所有課程elements傳給iterator

{% highlight java linenos %}
import java.util.Iterator;

public class ComputerCourse implements Aggregate{
  // 陣列元素
  private Course[] elements;
  // index索引
  private int index = 0;

  public ComputerCourse() {
    // 最多就10堂課
    elements = new Course[10];
  }

  //增加課程
  public void addCourse(String name) {
    // 最多就10堂課
    if (index < 10) {
      Course course = new Course(name);
      elements[index] = course;
      index++;
    }
  }

  // 取得Iterator
  @Override
  public Iterator createIterator() {
    // 要把elements傳入
    return new ComputerCourseIter(elements);
  }
}
{% endhighlight %}

實作陣列的Iterator
{% highlight java linenos %}
import java.util.Iterator;

public class ComputerCourseIter implements Iterator {
  Course[] elements;
  // 預設是0
  int index = 0;

  public ComputerCourseIter(Course[] elements) {
    this.elements = elements;
  }
  
  // 判斷是否有下一個元素
  @Override
  public boolean hasNext() {
    // index 超出大小 或 值是null，因為我們有建立10個陣列大小，沒設課程都會是null
    if (index >= elements.length || elements[index] == null) {
      return false;
    }
    return true;
  }
  
  // 取出下一個元素
  @Override
  public Object next() {
    // 先傳回值
    Course rtnObj = elements[index];
    // index移到下一個元素的索引
    index++;
    return rtnObj;
  }

  @Override
  public void remove() {
    // 目前沒此需求，先不寫
  }
}
{% endhighlight %}

測試
{% highlight java linenos %}
import java.util.Iterator;

public class Test {
  public static void main(String[] args) {
    ComputerCourse computerCourse = new ComputerCourse();
    computerCourse.addCourse("C++");
    computerCourse.addCourse("Java");
    computerCourse.addCourse("Android");
    computerCourse.addCourse("Swift");

    Iterator iter = computerCourse.createIterator();
    while(iter.hasNext()) {
      Course course = (Course) iter.next();
      System.out.println(course.getName());
    }
  }
}
{% endhighlight %}
```
C++
Java
Android
Swift
```

## List集合與Iterator
實作elements為List的集合與Iterator。

寫一個英語課程集合。

{% highlight java linenos %}
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class EnglishCourse implements Aggregate{
  private List<Course> elements;

  public EnglishCourse() {
    elements = new ArrayList<>();
  }

  public void addCourse(String name) {
    Course course = new Course(name);
    elements.add(course);
  }

  @Override
  public Iterator createIterator() {
    return new EnglishCourseIter(elements);
  }
}
{% endhighlight %}

實作Iterator
{% highlight java linenos %}
import java.util.Iterator;
import java.util.List;

public class EnglishCourseIter implements Iterator {
  private List<Course> elements;
  private int index = 0;
  public EnglishCourseIter(List<Course> elements) {
    this.elements = elements;
  }

  @Override
  public boolean hasNext() {
    if (index >= elements.size()){
      return false;
    }
    return true;
  }

  @Override
  public Object next() {
    // 先傳回
    Course rtnObj = elements.get(index);
    // 索引再++
    index++;
    return rtnObj;
  }

  @Override
  public void remove() {
    // 目前沒此需求，先不寫
  }
}
{% endhighlight %}

測試程式
{% highlight java linenos %}
import java.util.Iterator;

public class Test {
  public static void main(String[] args) {
    EnglishCourse englishCourse = new EnglishCourse();
    englishCourse.addCourse("自然發音");
    englishCourse.addCourse("文法");
    englishCourse.addCourse("會話");

    Iterator iter = englishCourse.createIterator();
    while(iter.hasNext()) {
      Course course = (Course) iter.next();
      System.out.println(course.getName());
    }
  }
}
{% endhighlight %}
```
自然發音
文法
會話
```