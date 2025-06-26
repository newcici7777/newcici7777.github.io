---
title: Arrays.sort()
date: 2025-06-26
keywords: java, Arrays sort
---
Prerequisites:

- [Comparator][1]

## sort()
預設是由小到大，可自訂排序方式由大到小。<br>
使用Arrays.sort()會影嚮陣列中原本的位置。<br>
{% highlight java linenos %}
int[] arr = {-1, 5, -2, 10};
System.out.println("排序前:" + Arrays.toString(arr));
Arrays.sort(arr);
System.out.println("排序後:" + Arrays.toString(arr));
{% endhighlight %}
```
排序前:[-1, 5, -2, 10]
排序後:[-2, -1, 5, 10]
```

### 自訂排序大小
- [Comparable與Comparator][1]
- [包裝類別Wrapper Classes][2]
- [匿名類別][3]

1. 陣列不能是基本型態，必須是包裝類別Wrapper Classes。
2. 使用匿名類別，實作Comparator介面。
3. 由小到大，是o1 - o2。
4. 由大到小，是o2 - o1。

語法
```
Arrays.sort(陣列, 自訂排序介面)
```

{% highlight java linenos %}
Integer[] arr = {-1, 5, -2, 10};
System.out.println("排序前:" + Arrays.toString(arr));
Arrays.sort(arr, new Comparator<Integer>() {
  @Override
  public int compare(Integer o1, Integer o2) {
    return o2 - o1;
  }
});
System.out.println("排序後:" + Arrays.toString(arr));
{% endhighlight %}
```
排序前:[-1, 5, -2, 10]
排序後:[10, 5, -1, -2]
```

## 自己寫MyArrays.sort()
- [氣泡排序][4]

自己寫一個MyArrays.sort(陣列, Comparator)

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    int[] arr = {-1, 5, 1, 10, 0, 7};
    // 由小到大
    MyArrays.sort(arr, new Comparator() {
      @Override
      public int compare(Object o1, Object o2) {
        // 自動拆箱
        int i1 = (Integer) o1;
        int i2 = (Integer) o2;
        return i1 - i2;
      }
    });
    System.out.println("排序後 : " + Arrays.toString(arr));
  }
}
class MyArrays {
  public static void sort(int[] arr, Comparator comparator) {
    int loop_count = arr.length - 1;
    for (int i = loop_count; i > 0; i--) {
      for (int j = 0; j < i; j++) {
        // arr[j] - arr[j + 1] 傳回 正數 ，代表arr[j] > arr[j + 1]
        // arr[j] - arr[j + 1] 傳回 負數 ，代表arr[j] < arr[j + 1]
        if(comparator.compare(arr[j] , arr[j + 1]) > 0) {
          // 交換
          int temp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = temp;
        }
      }
    }
  }
}
{% endhighlight %}

## 比較日期
有一個陣列，陣列的值是日期物件，排序這些日期物件。

### 日期物件
{% highlight java linenos %}
class MyDate2 {
  private int year;
  private int mon;
  private int day;

  public int getYear() {
    return year;
  }

  public int getMon() {
    return mon;
  }

  public int getDay() {
    return day;
  }

  public MyDate2(int year, int mon, int day) {
    this.year = year;
    this.mon = mon;
    this.day = day;
  }

  @Override
  public String toString() {
    return "\nMyDate2{" +
        "year=" + year +
        ", mon=" + mon +
        ", day=" + day +
        '}';
  }
}
{% endhighlight %}

### Comparator
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 透過匿名類別建立物件
    Comparator<MyDate2> comparator = new Comparator<MyDate2>() {
      @Override
      public int compare(MyDate2 o1, MyDate2 o2) {
        // 由小到大 o1 - o2
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

        int diff_day = o1.getDay() - o2.getDay();
        if (diff_day != 0) {
          return diff_day;
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

    // 陣列排序，把Comparator傳入。
    Arrays.sort(arr, comparator);
    System.out.println(Arrays.toString(arr));
  }
}
{% endhighlight %}
```
[
MyDate2{year=2000, mon=1, day=1}, 
MyDate2{year=2000, mon=3, day=1}, 
MyDate2{year=2000, mon=11, day=10}]
```
結果由小到大排列。

### 由大到小
想把排序結果由大到小排列，只要把o1與o2位置顛倒。
```
o2.getYear() - o1.getYear()
o2.getMon() - o1.getMon()
o2.getDay() - o1.getDay()
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
        // 由大到小
        int diff_y = o2.getYear() - o1.getYear();
        
        if (diff_y != 0) {
          return diff_y;
        }

        int diff_m = o2.getMon() - o1.getMon();
        if (diff_m != 0) {
          return diff_m;
        }

        int diff_day = o2.getDay() - o1.getDay();
        if (diff_day != 0) {
          return diff_day;
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
MyDate2{year=2000, mon=11, day=10}, 
MyDate2{year=2000, mon=3, day=1}, 
MyDate2{year=2000, mon=1, day=1}]
```
結果由大到小排列。

[1]: {% link _pages/java/compare.md %}
[2]: {% link _pages/java/wrap.md %}
[3]: {% link _pages/java/anonymous_class.md %}
[4]: {% link _pages/java/bubble_sort.md %}
