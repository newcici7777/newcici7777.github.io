---
title: Bubble Sort
date: 2025-06-19
keywords: Java, bubble sort
---
氣泡排序的重點:

陣列中，二個值比較，大的放後面。

## 第一輪
比5次。
1. 6,5先比較，6比5大，放後面。
2. 6,4比較，6比4大，放後面。
3. 6,3比較，6比3大，放後面。
4. 6,2比較，6比2大，放後面。
5. 6,1比較，6比1大，放後面。

![img]({{site.imgurl}}/dataStruct/bubbleSort1.jpg)  

交換的方式如下圖。

![img]({{site.imgurl}}/dataStruct/bubbleSort_temp.png) 

1. 準備一個暫存變數temp。
2. 6放在temp。
3. 5放在6的位置。
4. temp放在5的位置。

把以上的描述用程式寫死。
1. 比5次。
2. 都跟後面1個比，後面比較小進入下個步驟。
3. 跟後面的值交換。

{% highlight java linenos %}
  // 6, 5, 4, 3, 2, 1
  int[] arr = {6, 5, 4, 3, 2, 1};
  // 比5次
  for (int j = 0; j < 5; j++) {
    // 2. 都跟後面1個比，後面比較小進入下個步驟。
    if (arr[j] > arr[j + 1]) {
      // 3. 跟後面的值交換。
      int temp = arr[j];
      arr[j] = arr[j + 1];
      arr[j + 1] = temp;
    }
  }
  System.out.println(Arrays.toString(arr));
{% endhighlight %}
```
[5, 4, 3, 2, 1, 6]
```

## 第2輪
比4次。
1. 5,4先比較，5比4大，放後面。
2. 5,3比較，5比3大，放後面。
3. 5,2比較，5比2大，放後面。
4. 5,1比較，5比1大，放後面。

![img]({{site.imgurl}}/dataStruct/bubbleSort2.jpg) 

{% highlight java linenos %}
  for (int j = 0; j < 4; j++) {
    // 2. 都跟後面1個比。
    if (arr[j] > arr[j + 1]) {
      // 3. 比較大的，放後面
      int temp = arr[j];
      arr[j] = arr[j + 1];
      arr[j + 1] = temp;
    }
  }
  System.out.println(Arrays.toString(arr));
{% endhighlight %}
```
[4, 3, 2, 1, 5, 6]
```

## 第3輪
比3次。

1. 4,3比較，4比3大，放後面。
2. 4,2比較，4比2大，放後面。
3. 4,1比較，4比1大，放後面。

![img]({{site.imgurl}}/dataStruct/bubbleSort3.jpg) 

比3次
{% highlight java linenos %}
  // 比3次
  for (int j = 0; j < 3; j++) {
    // 2. 都跟後面1個比。
    if (arr[j] > arr[j + 1]) {
      // 3. 比較大的，放後面
      int temp = arr[j];
      arr[j] = arr[j + 1];
      arr[j + 1] = temp;
    }
  }
  System.out.println(Arrays.toString(arr));
{% endhighlight %}
```
[3, 2, 1, 4, 5, 6]
```

## 第4輪
比2次。

1. 3,2比較，3比2大，放後面。
2. 3,1比較，3比1大，放後面。

![img]({{site.imgurl}}/dataStruct/bubbleSort4.jpg) 

{% highlight java linenos %}
  // 比2次
  for (int j = 0; j < 2; j++) {
    // 2. 都跟後面1個比。
    if (arr[j] > arr[j + 1]) {
      // 3. 比較大的，放後面
      int temp = arr[j];
      arr[j] = arr[j + 1];
      arr[j + 1] = temp;
    }
  }
  System.out.println(Arrays.toString(arr));
{% endhighlight %}
```
[2, 1, 3, 4, 5, 6]
```

## 第5輪
比1次。

1. 2,1比較，2比1大，放後面。

![img]({{site.imgurl}}/dataStruct/bubbleSort5.jpg) 

{% highlight java linenos %}
  // 比1次
  for (int j = 0; j < 1; j++) {
    // 2. 都跟後面1個比。
    if (arr[j] > arr[j + 1]) {
      // 3. 比較大的，放後面
      int temp = arr[j];
      arr[j] = arr[j + 1];
      arr[j + 1] = temp;
    }
  }
  System.out.println(Arrays.toString(arr));
{% endhighlight %}
```
[1, 2, 3, 4, 5, 6]
```

## 重覆的程式碼
以下會出現5次內容一模一樣的程式碼，只有`j < ?`是有變化的，其它都沒有變化。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    int[] arr = {6, 5, 4, 3, 2, 1};

    // 比5次
    for (int j = 0; j < 5; j++) {
      // 2. 都跟後面1個比。
      if (arr[j] > arr[j + 1]) {
        // 3. 比較大的，放後面
        int temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
    System.out.println(Arrays.toString(arr));

    // 比4次
    for (int j = 0; j < 4; j++) {
      // 2. 都跟後面1個比。
      if (arr[j] > arr[j + 1]) {
        // 3. 比較大的，放後面
        int temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
    System.out.println(Arrays.toString(arr));

    // 比3次
    for (int j = 0; j < 3; j++) {
      // 2. 都跟後面1個比。
      if (arr[j] > arr[j + 1]) {
        // 3. 比較大的，放後面
        int temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
    System.out.println(Arrays.toString(arr));

    // 比2次
    for (int j = 0; j < 2; j++) {
      // 2. 都跟後面1個比。
      if (arr[j] > arr[j + 1]) {
        // 3. 比較大的，放後面
        int temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
    System.out.println(Arrays.toString(arr));

    // 比1次
    for (int j = 0; j < 1; j++) {
      // 2. 都跟後面1個比。
      if (arr[j] > arr[j + 1]) {
        // 3. 比較大的，放後面
        int temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
    System.out.println(Arrays.toString(arr));
  }
}
{% endhighlight %}

重覆5次的程式碼，一模一樣的程式碼，就由迴圈來省略重覆的程式碼。

`j < ?`的變化是由5, 4, 3, 2, 1，由大到小遞減。

所以外層迴圈要用由大到小，5 -> 4 -> 3 -> 2 -> 1。

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 6, 5, 4, 3, 2, 1
    int[] arr = {6, 5, 4, 3, 2, 1};
    // loop_count迴圈次數 5 -> 4 -> 3 -> 2 -> 1
    int loop_count = 5;
    for (int i = loop_count; i > 0; i--) {
      for (int j = 0; j < i; j++) {
        // 2. 都跟後面1個比。
        if (arr[j] > arr[j + 1]) {
          // 3. 比較大的，放後面
          int temp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = temp;
        }
      }
      System.out.println(Arrays.toString(arr));
    }
  }
}
{% endhighlight %}
```
[5, 4, 3, 2, 1, 6]
[4, 3, 2, 1, 5, 6]
[3, 2, 1, 4, 5, 6]
[2, 1, 3, 4, 5, 6]
[1, 2, 3, 4, 5, 6]
```

外層迴圈的作用是執行5次，所以下面的程式碼可以確保外層迴圈執行5次。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 6, 5, 4, 3, 2, 1
    int[] arr = {6, 5, 4, 3, 2, 1};
    // loop_count迴圈次數 5 -> 4 -> 3 -> 2 -> 1
    int loop_count = 5;
    for (int i = 0; i < loop_count; i++) {
      for (int j = 0; j < loop_count - i ; j++) {
        // 2. 都跟後面1個比。
        if (arr[j] > arr[j + 1]) {
          // 3. 比較大的，放後面
          int temp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = temp;
        }
      }
      System.out.println(Arrays.toString(arr));
    }
  }
}
{% endhighlight %}


外層迴圈次數是陣列大小 - 1。
{% highlight java linenos %}
int loop_count = arr.length - 1;;
{% endhighlight %}