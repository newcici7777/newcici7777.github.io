---
title: 快速排序
date: 2025-07-24
keywords: Java, QuickSort, QuickSort with 3-way partitioning, Dutch National Flag algorithm
---
## 分成三個區塊
傳統的快速排序太複雜，此處使用荷蘭國旗演算法，程式碼既容易理解也好寫。

荷蘭國旗有三個顏色，我們把陣列分成三個等份。

分別是:
1. 小於pivot
2. 等於pivot
3. 大於pivot

- S代表Small，小於pivot
- E代表Equal，等於pivot
- L代表Large，大於pivot

所以陣列會顯示如下:<br>
SSSSSEEEEEELLLLLLL<br>


## 變數介紹
介紹完Small,Equal,Large，接下來要介紹變數。

small變數以「前」的都是Small區域，小於pivot。<br>
large變數以「後」的都是Large區域，大於pivot。<br>
i變數作為移動(走訪)陣列中的每個元素，當i變數「大於」large變數，就離開迴圈，因為「小於」、「等於」、「大於」，三個區域已分類完畢。<br>
{% highlight java linenos %}
// 進入迴圈的條件 i <= large
// 離開迴圈的條件 i > large
while (i <= large) {
}
{% endhighlight %}

## 交換

## 完整程式碼
{% highlight java linenos %}
public class QuickSort2 {
  public static void main(String[] args) {
    //int[] arr = {22,55,0,-2,-1};
    int len = 10;
    int[] arr = new int[len];
    for (int i = 0; i < len; i++) {
      arr[i] = (int) (Math.random() * 100);
    }
    System.out.println("排序前:" + Arrays.toString(arr));
    quicksort(arr, 0, arr.length - 1);
    System.out.println("排序後:" + Arrays.toString(arr));
  }

  public static void quicksort(int[] arr, int left, int right) {
    if (left >= right) return;
    int pivotIndex = (right + left) / 2;
    int pivot = arr[pivotIndex];
    int small = left;
    int large = right;
    int i = small;
    while (i <= large) {
      if (arr[i] < pivot) {
        int temp = arr[small];
        arr[small] = arr[i];
        arr[i] = temp;
        small++;
        i++;
      } else if (arr[i] > pivot) {
        int temp = arr[large];
        arr[large] = arr[i];
        arr[i] = temp;
        large--;
      } else {
        i++;
      }
    }
    quicksort(arr, left, small - 1);
    quicksort(arr, large + 1, right);
  }
}
{% endhighlight %}
