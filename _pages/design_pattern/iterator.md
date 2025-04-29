---
title: Iterator
date: 2025-04-29
keywords: Java, Design patterns, Iterator Pattern
---
以下的類別圖分二類，左邊是集合Aggregate，右邊是Iterator疊代器。

![img]({{site.imgurl}}/pattern/iter1.png)

## Aggregate
Aggregate介面有createIterator()方法必須實作。

實作Aggregate類別，成員屬性有elements，可以是陣列、List、任何集合類別。

createIterator()取得跟elements類別相關的Iterator，比如elements是陣列，就要取得陣列的Iterator，elements是List，就要取得List的Iterator，elements是Map，就要取得Map的Iterator。

以上提到的Iterator自己要實作。

## Iterator
Iterator介面有hasNext()、next()、remove()三個方法要實作。

hasNext(): 有沒有下一個元素(element)？傳回值型態為boolean

next(): 傳回下一個元素(element)。

remove(): 刪除元素

Iterator使用系統就有的，不另外自己寫。

實作Iterator類別，這個要自己實作。

## 陣列集合與Iterator

下圖中，實作Aggregate類別的elements的類型已換成陣列。

![img]({{site.imgurl}}/pattern/iter2.png)

寫一個電腦課程的集合與iterator

課程Course
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

Component介面
{% highlight java linenos %}
public interface Component {
  Iterator createIterator();
}
{% endhighlight %}

電腦課程集合  
要實作createIterator()方法，並把elements傳給iterator
{% highlight java linenos %}
import java.util.Iterator;

public class ComputerCourse implements Component{
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

  // 覆寫取得Iterator
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
    // 傳回值
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

main主程式
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

寫一個英語課程。

{% highlight java linenos %}
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class EnglishCourse implements Component{
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
    Course rtnObj = elements.get(index);
    index++;
    return rtnObj;
  }

  @Override
  public void remove() {
    // 目前沒此需求，先不寫
  }
}
{% endhighlight %}

main主程式
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