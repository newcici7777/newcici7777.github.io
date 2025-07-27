---
title: 合併排序
date: 2025-07-23
keywords: Java, MergeSort
---
Prerequisites:

- [遞迴][1]

步驟:<br>
1. 先切割陣列到只剩下一個元素就離開遞迴。
2. 合併

## 切割

## 完整程式碼
{% highlight java linenos %}
public class MergeSort {
  public static void main(String[] args) {
    int len = 5;
    int[] arr = new int[len];
    for (int i = 0; i < len; i++) {
      arr[i] = (int) (Math.random() * 100);
    }
    System.out.println("排序前:" + Arrays.toString(arr));
    int[] copyarr = new int[arr.length];
    cut(arr, 0, arr.length - 1, copyarr);
    System.out.println("排序後:" + Arrays.toString(arr));
  }

  public static void cut(int[] arr, int left, int right, int[] copyarr) {
    if (left >= right) {
      return;
    }
    int mid = (left + right) / 2;
    cut(arr, left, mid, copyarr);
    cut(arr, mid + 1, right, copyarr);
    merge(arr, left, right, mid, copyarr);
  }

  public static void merge(int[] arr, int left, int right, int mid, int[] copyarr) {
    int t = 0;
    int i = left;
    int j = mid + 1;
    while (i <= mid && j <= right) {
      if (arr[i] < arr[j]) {
        copyarr[t++] = arr[i];
        i++;
      } else {
        copyarr[t++] = arr[j];
        j++;
      }
    }

    while (j <= right) {
      copyarr[t++] = arr[j++];
    }

    while (i <= mid) {
      copyarr[t++] = arr[i++];
    }

    for (int k = 0; k < t; k++) {
      arr[left++] = copyarr[k];
    }
  }
}
{% endhighlight %}

[1]: {% link _pages/java/recursive.md %}
