---
title: 二分搜尋
date: 2025-08-02
keywords: java, Binary Search
---
遞迴離開條件為r < l，假設要找尋5，最後一次是mid = 0，r與l是相等的，但`arr[mid] != 5`，因為要找的5比`arr[mid] = 10`小，因此r會為mid - 1 = 0 - 1 = -1。<br>

```
lr 
↓   
10, 15, 20, 25, 30
```

```
r   l 
↓   ↓
10, 15, 20, 25, 30
```

假設要找尋35，最後一次是mid = 4，r與l是相等的，但`arr[mid] != 35`，因為要找的35比`arr[mid] = 30`小，因此l會為mid + 1 = 4 + 1 = 5。<br>

```
                lr 
                ↓   
10, 15, 20, 25, 30
```

```
                r   l 
                ↓   ↓
10, 15, 20, 25, 30
```
## 完整程式碼
{% highlight java linenos %}
public class BinarySearch {
  public static void main(String[] args) {
    BinarySearch bs = new BinarySearch();
    int[] arr = {10, 15, 20, 25, 30};
    int findIndex = bs.binarySearch(arr, 0, arr.length - 1, 25);
    System.out.println("findIndex = " + findIndex);
    int findIndex2 = bs.binarySearch2(arr, 20);
    System.out.println("findIndex2 = " + findIndex2);
  }
  // 遞迴
  public int binarySearch(int[] arr, int left, int right, int findValue) {
    if (left > right) return -1;
    int mid = (left + right) / 2;
    if (arr[mid] == findValue) {
      return mid;
    } else if (findValue < arr[mid]) {
      return binarySearch(arr, left, mid - 1, findValue);
    } else {
      return binarySearch(arr, mid + 1, right, findValue);
    }
  }

  public int binarySearch2(int[] arr, int findValue) {
    int left = 0;
    int right = arr.length - 1;
    // 進入迴圈的條件，l與r會指向相同位置
    // 離開迴圈的條件，l > r
    while (left <= right) {
      int mid = (left + right) / 2;
      if (arr[mid] == findValue) {
        return mid;
      } else if (findValue < arr[mid]) {
        right = mid - 1;
      } else {
        left = mid + 1;
      }
    }
    return -1;
  }
}
{% endhighlight %}