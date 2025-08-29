---
title: Heap Sort
date: 2025-08-29
keywords: java, Heap sort, complete binary tree
---
Heap Sort，中文有堆積排序或累堆排序。<br>
使用完整二元樹與陣列建立堆積樹與排序。<br>

## 完整二元樹
完整二元樹(Complete Binary Tree)，由上而下由左而右(參考下圖箭頭方向)，中間不會有空缺，逐一把節點一個一個放入陣列中。<br>
![img]({{site.imgurl}}/java_datastruct/com_b_tree1.png)<br>

![img]({{site.imgurl}}/java_datastruct/com_b_tree2.png)<br>
對映的陣列如下:<br>
```
[20, 15, 5, 1, 100]
```

![img]({{site.imgurl}}/java_datastruct/com_b_tree3.png)<br>

對映的陣列如下:<br>
```
[20, 15, 5, 1, 100, 6]
```

![img]({{site.imgurl}}/java_datastruct/com_b_tree4.png)<br>
對映的陣列如下:<br>
```
[20, 15, 5, 1, 100, 6, 30]
```

若中間有空缺，就不是完整二元樹。<br>
以下缺少第二層的右子樹，不是完整二元樹。<br>
![img]({{site.imgurl}}/java_datastruct/com_b_tree5.png)<br>

以下有少一個節點，有空缺，不是完整二元樹。
![img]({{site.imgurl}}/java_datastruct/com_b_tree6.png)<br>

## 陣列索引與樹
下圖，加上對映的陣列索引。<br>
![img]({{site.imgurl}}/java_datastruct/com_b_tree7.png)<br>

陣列:<br>

|索引|0|1|2|3|4|
|數值|20|15|5|1|100|

左子節點公式，i為「父」節點索引。<br>
```
2 * i + 1
```

右子節點公式，i為「父」節點索引。
```
2 * i + 2
```

找出父節點公式，i為「子」節點索引。
```
(i - 1) / 2
```

假如我們要找父節點索引為1，值是15，它的左右子節點，要如何找？<br>

左子節點
```
2 * 1 + 1 = 3
```

|索引|0|1|2|<span class="markline">3</span>|4|
|數值|20|15|5|<span class="markline">1</span>|100|

右子節點
```
2 * 1 + 2 = 4
```

|索引|0|1|2|3|<span class="markline">4</span>|
|數值|20|15|5|1|<span class="markline">100</span>|

找出索引為4的父節點，無條件去掉小數點。
```
(4 - 1) / 2 = 1
```

|索引|0|<span class="markline">1</span>|2|3|4|
|數值|20|15|5|1|100|

## 最大堆積樹與最小堆積樹
### 最大堆積樹
根節點都比左右子樹大。<br>
![img]({{site.imgurl}}/java_datastruct/heapmax.png)<br>

### 最小堆積樹
根節點都比左右子樹小。<br>
![img]({{site.imgurl}}/java_datastruct/heapmin.png)<br>

## 建立最大堆積樹
利用以下公式找到最後一個

## 建立最大堆積樹
最大堆積樹由下往上，把最大值搬到根節點。<br>
所以以下步驟由下往上找。<br>
### 最後一個非葉子父節點
下圖中，索引1，值為15，就是最後一個非葉子父節點。<br>
![img]({{site.imgurl}}/java_datastruct/heap_lastp.png)<br>

索引|0|1|2|3|4|
|數值|20|15|5|1|100|

#### 方法1:最後一個葉子的父節點
先前有提過找到「子」索引的父節點。
```
(i - 1) / 2
```

陣列中的最後一個元素，一定是葉子節點，可透過它來找父節點。<br>
以下公式為「整數」除法，無條件捨去小數。<br>
```
(arr.length - 1 - 1) / 2
(5 - 1 - 1) / 2 = 1
```

#### 方法2:陣列大小/2 - 1
陣列大小除，左半邊都是父節點，右半邊都是子節點。<br>
陣列大小5除2 = 2，小於2不包含2都是左半邊，左半邊都是父節點，大於等於2，右半邊都是子節點。<br>

|父或葉子|父|父|葉|葉|葉|
|索引    |0|1 |2 |3 |4|
|數值    |20|15|5|1|100|

如果要取得最後一個父節點，也就要把陣列大小/2，再減1，就會取得最後一個父節點。<br>
```
(arr.length / 2) - 1
(5 / 2) - 1 = 1
```

### 左右子節點比父節點大就交換。
1. 左右子節點先比較誰比較大
2. 比較大的子節點，再跟父節點比大小，若子節點比父節點大，就交換。

![img]({{site.imgurl}}/java_datastruct/heap_swap1.png)<br>

### 倒數第二個父節點
先前有提過，陣列大小/2 = 第一個葉子節點，陣列大小/2，左半邊都是父節點，右半邊都是葉子節點。

剛才處理完最後一個父節點，索引為1，請問倒數第2個父節點怎麼求？<br>
也就是索引1 - 1 = 0 ，就會得到倒數第2個父節點，索引為0。<br>

|父或葉子|父|父|葉|葉|葉|
|索引    |0|1 |2 |3 |4|
|數值    |20|15|5|1|100|

索引0，先看左右子節點有沒有比索引0的值大，有的話就交換。<br>
![img]({{site.imgurl}}/java_datastruct/heap_swap2.png)<br>

目前最大堆積樹已完成。

## 把根節點放在陣列最後一個索引位置




{% highlight java linenos %}
public class HeapSort {
  public static void main(String[] args) {
    int[] arr = {20, 15, 5, 1, 100};
    preOrder(0, arr);
    for (int i = arr.length / 2 - 1; i >= 0; i--) {
      adjust(arr, i , arr.length);
    }
    System.out.println();
    preOrder(0, arr);
    for (int i = arr.length - 1; i > 0 ; i--) {
      int temp = arr[i];
      arr[i] = arr[0];
      arr[0] = temp;
      adjust(arr, 0, i);
    }
    System.out.println();
    System.out.println(Arrays.toString(arr));
  }

  public static void adjust(int[] arr, int i, int len) {
    int temp = arr[i];
    for (int k = i * 2 + 1; k < len; k++) {
      if (k + 1 < len && arr[k] < arr[k + 1]) {
        k++;
      }
      if (temp < arr[k]) {
        arr[i] = arr[k];
        i = k;
      } else {
        break;
      }
    }
    arr[i] = temp;
  }

  public static void preOrder(int i, int[] arr) {
    System.out.print(arr[i] + ",");
    if (i * 2 + 1 < arr.length) {
      preOrder(i * 2 + 1, arr);
    }
    if (i * 2 + 2 < arr.length) {
      preOrder(i * 2 + 2, arr);
    }
  }
}
{% endhighlight %}