---
title: Comparable與Comparator
day: 2025-05-26
keywords: Java, Comparable, Comparator
---
Prerequisites:

- [介面][1]
- [泛型][2]
- [泛型介面][3]
- [氣泡排序][6]

## 由小到大 由大到小
假設有陣列如下:<br>
1, 2, 3, 4, 5 <br>

### 由小到大
由小到大是指「目前位置」與「下一個位置」進行比較，比較方式是「目前位置」減「下一個位置」。

```
1, 2, 3, 4, 5 
↑  ↑
目 下
前 一
位 個
置 位
   置
```
1 - 2 = -1

傳回結果為-1，代表「目前位置」比「下一個位置」小，以[氣泡排序][6]來說，就不用交換。<br>

[氣泡排序][6]
```
if (比較結果 > 0) {
  交換
}
```

### 由大到小
比較方式是「下一個位置」減「目前位置」。

```
1, 2, 3, 4, 5 
↑  ↑
目 下
前 一
位 個
置 位
   置
```
「下一個位置」減「目前位置」 <br>
2 - 1 = 1

傳回結果為1，代表「下一個位置」比「目前位置」大，以[氣泡排序][6]來說，比較結果 > 0要交換。<br>

[氣泡排序][6]
```
if (比較結果 > 0) {
  交換
}
```

交換之後變成下面這樣，2與1就完成由大到小排序。
```
2, 1, 3, 4, 5 
```

## Comparable介面
Comparable是一個介面，有一個compareTo()方法，功用是比較大小。

Comparable介面原始檔
{% highlight java linenos %}
public interface Comparable<T> {
    public int compareTo(T o);
}
{% endhighlight %}

### compareTo(o)參數
每一個方法都有隱藏參數this，this就是目前位置物件，而o參數，是下一個位置的物件。

```
1, 2, 3, 4, 5 
↑  ↑
目 下
前 一
位 個
置 位
   置
```

### 比較
由小到大，this - o <br>
由大到小，o - this <br>

this是目前位置物件，o是下一個位置物件。

以下是由小到大。
{% highlight java linenos %}
class MyDate implements Comparable<MyDate> {
  private int year;
  private int mon;
  private int day;

  public MyDate(int year, int mon, int day) {
    this.year = year;
    this.mon = mon;
    this.day = day;
  }

  @Override
  public int compareTo(MyDate o) {
    // 由小到大 this - o
    int diff_y = this.year - o.year;
    // 不相等代表比較出結果，直接傳回比較結果
    // 之後的程式就沒必要再比較。
    if (diff_y != 0) {
      return diff_y;
    }

    int diff_m = this.mon - o.mon;
    if (diff_m != 0) {
      return diff_m;
    }

    int diff_day = this.day - o.day;
    if (diff_day != 0) {
      return diff_day;
    }
    return 0;
  }
}
{% endhighlight %}

### 比較日期大小
正數代表myDate > anotherDate <br>
負數代表myDate < anotherDate <br>
0代表myDate == anotherDate <br>
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    MyDate myDate = new MyDate(2000, 11, 11);
    MyDate anotherDate = new MyDate(2000, 11, 5);
    int res = myDate.compareTo(anotherDate);
    System.out.println(res);
    System.out.println("==========");

    myDate = new MyDate(2000, 11, 1);
    anotherDate = new MyDate(2000, 11, 10);
    res = myDate.compareTo(anotherDate);
    System.out.println(res);
    System.out.println("==========");

    myDate = new MyDate(2000, 11, 11);
    anotherDate = new MyDate(2000, 11, 11);
    res = myDate.compareTo(anotherDate);
    System.out.println(res);
  }
}
{% endhighlight %}
```
6
==========
-9
==========
0
```

### 其它:泛型與Comparable
在[泛型介面][3]的文章中，有提到實作泛型介面時要指定泛型類型，以下程式碼指定的泛型類型為MyDate，Intellij實作介面時會把MyDate的類型替代原本的T。
{% highlight java linenos %}
class MyDate implements Comparable<MyDate> {
  @Override
  public int compareTo(MyDate o) {
    return 0;
  }
}
{% endhighlight %}

---------------------------------

## Comparator

- [匿名類別][4]
- [Lambda][5]

Comparator可以透過[匿名類別][4]、[Lambda][5]，建立物件，只要實作比較的內容，就可單獨使用。

MyDate2提供getYear()、getMon()、getDay()，供其它類別使用。
{% highlight java linenos %}
class MyDate2 {
  private int year;
  private int mon;
  private int day;

  public MyDate2(int year, int mon, int day) {
    this.year = year;
    this.mon = mon;
    this.day = day;
  }

  public int getYear() {
    return year;
  }

  public int getMon() {
    return mon;
  }

  public int getDay() {
    return day;
  }
}
{% endhighlight %}

### Comparator.compare(o1,o2)
o1代表目前位置，o2代表下一個位置。

由小到大，o1 - o2 <br>
由大到小，o2 - o1 <br>

o1是目前位置物件，o2是下一個位置物件。

```
1, 2, 3, 4, 5 
↑  ↑
目 下
前 一
位 個
置 位
   置
```
### 比較日期大小
正數代表myDate2 > another <br>
負數代表myDate2 < another <br>
0代表myDate2 == another <br>

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    MyDate2 myDate2 = new MyDate2(2000, 11, 11);
    MyDate2 another = new MyDate2(2000, 11, 10);
    // 透過匿名類別建立物件
    Comparator<MyDate2> comparator = new Comparator<MyDate2>() {
      @Override
      public int compare(MyDate2 o1, MyDate2 o2) {
        // 由小到大，o1 - o2
        int diff_y = o1.getYear() - o2.getYear();
        if (diff_y != 0) {
          return diff_y;
        }

        int diff_m = o1.getMon() - o2.getMon();
        if (diff_m != 0) {
          return diff_m;
        }

        int diff_day = o1.getDay() - o2.getDay();
        if (diff_day != 0) {
          return diff_day;
        }
        return 0;
      }
    };
    int result = comparator.compare(myDate2, another);
    System.out.println(result);
  }
}
{% endhighlight %}
```
1
```


[1]: {% link _pages/java/interface.md %}
[2]: {% link _pages/java/generics.md %}
[3]: {% link _pages/java/generics_interface.md %}
[4]: {% link _pages/java/anonymous_class.md %}
[5]: {% link _pages/java/lambda.md %}
[6]: {% link _pages/java/bubble_sort.md %}