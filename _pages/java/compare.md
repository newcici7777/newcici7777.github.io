---
title: Comparable與Comparator
date: 2025-05-26
keywords: Java, Comparable, Comparator
---
Prerequisites:

- [介面][1]
- [泛型][2]
- [泛型介面][3]

## 傳回值
Comparable, Comparator都是比較介面，傳回值固定0、正數、負數。

0代表等於。

## Comparable介面
Comparable是一個介面，有一個compareTo()方法，功用是比較大小。

Comparable介面原始檔
{% highlight java linenos %}
public interface Comparable<T> {
    public int compareTo(T o);
}
{% endhighlight %}

### 使用方法
在[泛型介面][3]的文章中，有提到實作泛型介面時要指定泛型類型，以下程式碼指定的泛型類型為MyDate，Intellij實作介面時會把MyDate的類型替代原本的T。
{% highlight java linenos %}
class MyDate implements Comparable<MyDate> {
  @Override
  public int compareTo(MyDate o) {
    return 0;
  }
}
{% endhighlight %}

### compareTo(MyDate)參數
每一個方法都有隱藏參數this，this就是自己本身物件，而MyDate參數，是外部傳來要比較的物件，不是自己本身。

### 比較
比較的方法內容自己寫。

比較方法:
- 自己(this)比參數大，傳正數。
- 自己(this)比參數小，傳負數。
- 二者相等傳0。

以下是使用相減作為比較結果，注意，如果是用o.year - this.year，這樣結果就不會符合比較方法。
{% highlight java linenos %}
class MyDate implements Comparable<MyDate> {
  private int year;
  private int mon;
  private int date;

  public MyDate(int year, int mon, int date) {
    this.year = year;
    this.mon = mon;
    this.date = date;
  }

  @Override
  public int compareTo(MyDate o) {
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

    int diff_date = this.date - o.date;
    if (diff_date != 0) {
      return diff_date;
    }
    return 0;
  }
}
{% endhighlight %}

測試
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
### JDK8 String compareTo
String 實作Comparable\<String\>。

String字串是用final char\[\]陣列儲存，注意！是final，代表不能更改為其它字串陣列。
{% highlight java linenos %}
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char value[];
}
{% endhighlight %}

compareTo()
{% highlight java linenos %}
public int compareTo(String anotherString) {
    // value是本身的字串陣列
    int len1 = value.length;
    int len2 = anotherString.value.length;
    
    // 取得二個字串最小長度
    int lim = Math.min(len1, len2);

    char v1[] = value;
    char v2[] = anotherString.value;

    int k = 0;
    // 2個字串在最小長度下，比較內容
    while (k < lim) {
        char c1 = v1[k];
        char c2 = v2[k];
        if (c1 != c2) {
            return c1 - c2;
        }
        k++;
    }
    // 不同長度回傳 長度相減
    return len1 - len2;
}
{% endhighlight %}

#### 情況1
```
value = abcdefg
another = abdd
```
結果為c(99) - d(100) = -1

#### 情況2
以下二個字串前4個字元都一樣，只有長度不同。
```
value = abcdefg
another = abcd
```
傳回3

#### 情況3
以下二個字串前4個字元都一樣，只有長度不同。
```
value = abcde
another = abcdfg
```
傳回-3

## Comparator

- [匿名類別][4]
- [Lambda][5]

Comparator可以透過[匿名類別][4]、[Lambda][5]，建立物件，只要實作比較的內容，就可單獨使用。

MyDate2提供getYear()、getMon()、getDate()，供其它類別使用。
{% highlight java linenos %}
class MyDate2 {
  private int year;
  private int mon;
  private int date;

  public MyDate2(int year, int mon, int date) {
    this.year = year;
    this.mon = mon;
    this.date = date;
  }

  public int getYear() {
    return year;
  }

  public int getMon() {
    return mon;
  }

  public int getDate() {
    return date;
  }
}
{% endhighlight %}

建立比較器物件，進行比較，compare方法跟Comparable邏輯一模一樣。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    MyDate2 myDate2 = new MyDate2(2000, 11, 11);
    MyDate2 another = new MyDate2(2000, 11, 10);
    // 透過匿名類別建立物件
    Comparator<MyDate2> comparator = new Comparator<MyDate2>() {
      @Override
      public int compare(MyDate2 o1, MyDate2 o2) {
        int diff_y = o1.getYear() - o2.getYear();
        if (diff_y != 0) {
          return diff_y;
        }

        int diff_m = o1.getMon() - o2.getMon();
        if (diff_m != 0) {
          return diff_m;
        }

        int diff_date = o1.getDate() - o2.getDate();
        if (diff_date != 0) {
          return diff_date;
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

## Arrays.sort()排序
### sort()原始碼
第2個參數是把Comparator傳入，就可以自訂排序的方法。
{% highlight java linenos %}
public static <T> void sort(T[] a, Comparator<? super T> c) {
  if (c == null) {
    sort(a);
  } else {
    if (LegacyMergeSort.userRequested)
        legacyMergeSort(a, c);
    else
        TimSort.sort(a, 0, a.length, c, null, 0, 0);
  }
}
{% endhighlight %}

### Arrays.sort()排序與Comparator
MyDate2增加toString()。
{% highlight java linenos %}
class MyDate2 {
  private int year;
  private int mon;
  private int date;

  public int getYear() {
    return year;
  }

  public int getMon() {
    return mon;
  }

  public int getDate() {
    return date;
  }

  public MyDate2(int year, int mon, int date) {
    this.year = year;
    this.mon = mon;
    this.date = date;
  }

  @Override
  public String toString() {
    return "\nMyDate2{" +
        "year=" + year +
        ", mon=" + mon +
        ", date=" + date +
        '}';
  }
}
{% endhighlight %}

排序使用自訂Comparator。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 透過匿名類別建立物件
    Comparator<MyDate2> comparator = new Comparator<MyDate2>() {
      @Override
      public int compare(MyDate2 o1, MyDate2 o2) {
        int diff_y = o1.getYear() - o2.getYear();
        // 不相等代表比較出結果，直接傳回比較結果
        // 之後的程式就沒必要再比較。
        if (diff_y != 0) {
          return diff_y;
        }

        int diff_m = o1.getMon() - o2.getMon();
        if (diff_m != 0) {
          return diff_m;
        }

        int diff_date = o1.getDate() - o2.getDate();
        if (diff_date != 0) {
          return diff_date;
        }
        return 0;
      }
    };

    // 建立Array，裡面的內容是沒按照年月日順序
    MyDate2[] arr = {
        new MyDate2(2000, 11, 10),
        new MyDate2(2000, 1, 1),
        new MyDate2(2000, 3, 1)
    };
    // 陣列排序，把自訂Comparator傳入。
    Arrays.sort(arr, comparator);
    System.out.println(Arrays.toString(arr));
  }
}
{% endhighlight %}
```
[
MyDate2{year=2000, mon=1, date=1}, 
MyDate2{year=2000, mon=3, date=1}, 
MyDate2{year=2000, mon=11, date=10}]
```
結果由小到大排列。

### 由大到小
想把排序結果由大到小排列，只要把o1與o2位置顛倒。
```
o2.getYear() - o1.getYear()
o2.getMon() - o1.getMon()
o2.getDate() - o1.getDate()
```

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    MyDate2 myDate2 = new MyDate2(2000, 11, 11);
    MyDate2 another = new MyDate2(2000, 11, 10);
    // 透過匿名類別建立物件
    Comparator<MyDate2> comparator = new Comparator<MyDate2>() {
      @Override
      public int compare(MyDate2 o1, MyDate2 o2) {
        int diff_y = o2.getYear() - o1.getYear();
        // 不相等代表比較出結果，直接傳回比較結果
        // 之後的程式就沒必要再比較。
        if (diff_y != 0) {
          return diff_y;
        }

        int diff_m = o2.getMon() - o1.getMon();
        if (diff_m != 0) {
          return diff_m;
        }

        int diff_date = o2.getDate() - o1.getDate();
        if (diff_date != 0) {
          return diff_date;
        }
        return 0;
      }
    };
    MyDate2[] arr = {
        new MyDate2(2000, 11, 10),
        new MyDate2(2000, 1, 1),
        new MyDate2(2000, 3, 1)
    };
    Arrays.sort(arr, comparator);
    System.out.println(Arrays.toString(arr));
  }
}
{% endhighlight %}
```
[
MyDate2{year=2000, mon=11, date=10}, 
MyDate2{year=2000, mon=3, date=1}, 
MyDate2{year=2000, mon=1, date=1}]
```
結果由大到小排列。

[1]: {% link _pages/java/interface.md %}
[2]: {% link _pages/java/generics.md %}
[3]: {% link _pages/java/generics_interface.md %}
[4]: {% link _pages/java/anonymous_class.md %}
[5]: {% link _pages/java/lambda.md %}