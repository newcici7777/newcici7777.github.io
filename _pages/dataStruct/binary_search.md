---
title: 二分搜尋
date: 2025-08-02
keywords: java, Binary Search
---
## 二分搜尋觀念
排序好的陣列，才能用二分搜尋。<br>
一個陣列，每次切一半，取得中間數，判斷中間數是否要找到數字，若不是，搜尋的數字比中間數大，到右半邊陣列找，搜尋的數字比中間數小，到左半邊陣列找，左半邊與右半邊的陣列都「不包含中間數」。<br>

## 二分搜尋的時間複雜度
時間複雜度為O($\log_2{n}$)。<br>
因為每一次都把搜尋範圍就會減半，範圍減半就是除2，除2就是log，以2為底數。<br>

若有10筆資料，n = 10，就會進行$\log_2{10}$，約切割4次，$2^4 = 16$，16 > 10，2的4次方就是切割次數。<br>

假如有10資料，範圍減半4次。<br>
$10 \div 2 = 5$ <br>
$5 \div 2 = 2$ <br>
$2 \div 2 = 1$ <br>
$1 \div 2 = 0$ <br>

## 離開遞迴條件與離開迴圈條件
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
    // 離開遞迴條件
    if (left > right) return -1;
    // 取得中間位置
    int mid = (left + right) / 2;
    // 中間位置的值等於findValue
    if (arr[mid] == findValue) {
      return mid;
    } else if (findValue < arr[mid]) {
      // 搜尋範圍變左半邊，不包含mid
      return binarySearch(arr, left, mid - 1, findValue);
    } else {
      // 搜尋範圍變右半邊，不包含mid
      return binarySearch(arr, mid + 1, right, findValue);
    }
  }

  public int binarySearch2(int[] arr, int findValue) {
    int left = 0;
    // right為陣列最後一個元素的位置
    int right = arr.length - 1;
    // 進入迴圈的條件，l與r會指向相同位置
    // 離開迴圈的條件，l > r
    while (left <= right) {
      // 中間位置
      int mid = (left + right) / 2;
      // 如果中間位置的值等於findValue
      if (arr[mid] == findValue) {
        return mid;  // 找到了
      } else if (findValue < arr[mid]) {
      	// 如果findValue小於中間位置的值
      	// 範圍在左半邊，不包含mid
        right = mid - 1;
      } else {
      	// 如果findValue大於中間位置的值
      	// 範圍在右半邊，不包含mid
        left = mid + 1;
      }
    }
    return -1;
  }
}
{% endhighlight %}

## 處理要找到多個相同數字
考題常常出現要尋找多個相同數字，找「最左邊」相同數字索引、找「最右邊」的相同數字索引，找最後一個小於「最左邊」相同數字的索引，找第一個大於「最右邊」相同數字的索引。

最左邊相同數字索引1
```
    ↓
10, 20, 20, 20, 30
```

最右邊相同數字索引3
```
            ↓
10, 20, 20, 20, 30
```

最後一個「小於」最左邊相同數字20的索引0。
```
 ↓
10, 20, 20, 20, 30
```

第一個「大於」最右邊相同數字20的索引4。
```
                ↓
10, 20, 20, 20, 30
```

為了處理這種題目，mid找到後，先不要傳回mid，先由mid左邊(mid-1)與mid右邊(mid+1)，二邊擴展去尋找相同的數，並把索引位置加入list中，直到不是相同的數才離開迴圈。

可根據得到的list，由小到大排序，就可以處理上述的題目。

{% highlight java linenos %}
import java.util.ArrayList;
import java.util.List;

public class BinarySearch2 {
  public static void main(String[] args) {
    BinarySearch2 bs2 = new BinarySearch2();
    int[] arr = {10, 20, 20, 20, 30};
    List rtnList = bs2.binarySearch(arr, 0, arr.length - 1, 20);
    System.out.println("findIndex = " + rtnList);
    List rtnList2 = bs2.binarySearch2(arr, 20);
    System.out.println("findIndex2 = " + rtnList2);
  }
  // 遞迴
  public List binarySearch(int[] arr, int left, int right, int findValue) {
    List<Integer> rtnList = new ArrayList();
    // 離開遞迴條件
    if (left > right) return rtnList;
    // 取得中間位置
    int mid = (left + right) / 2;
    // 中間位置的值等於findValue
    if (arr[mid] == findValue) {
      // list加上mid索引
      rtnList.add(mid);
      // 向左找相同的數值
      int left_mid = mid - 1;
      while (true) {
        // 相同就往左繼續找
        if (arr[left_mid] == findValue) {
          rtnList.add(left_mid);
          left_mid--;
        } else {
          // 不相同就離開迴圈
          break;
        }
      }
      // 向右找相同的數值
      int right_mid = mid + 1;
      while (true) {
        // 相同就往右繼續找
        if (arr[right_mid] == findValue) {
          rtnList.add(right_mid);
          right_mid++;
        } else {
          // 不相同就離開迴圈
          break;
        }
      }
      return rtnList;
    } else if (findValue < arr[mid]) {
      // 搜尋範圍變左半邊，不包含mid
      return binarySearch(arr, left, mid - 1, findValue);
    } else {
      // 搜尋範圍變右半邊，不包含mid
      return binarySearch(arr, mid + 1, right, findValue);
    }
  }

  public List binarySearch2(int[] arr, int findValue) {
    List<Integer> rtnList = new ArrayList();
    int left = 0;
    // right為陣列最後一個元素的位置
    int right = arr.length - 1;
    // 進入迴圈的條件，l與r會指向相同位置
    // 離開迴圈的條件，l > r
    while (left <= right) {
      // 中間位置
      int mid = (left + right) / 2;
      // 如果中間位置的值等於findValue
      if (arr[mid] == findValue) {
        // list加上mid索引
        rtnList.add(mid);
        // 向左找相同的數值
        int left_mid = mid - 1;
        while (true) {
          // 相同就往左繼續找
          if (arr[left_mid] == findValue) {
            rtnList.add(left_mid);
            left_mid--;
          } else {
            // 不相同就離開迴圈
            break;
          }
        }
        // 向右找相同的數值
        int right_mid = mid + 1;
        while (true) {
          // 相同就往右繼續找
          if (arr[right_mid] == findValue) {
            rtnList.add(right_mid);
            right_mid++;
          } else {
            // 不相同就離開迴圈
            break;
          }
        }
        return rtnList;  // 找到了
      } else if (findValue < arr[mid]) {
        // 如果findValue小於中間位置的值
        // 範圍在左半邊，不包含mid
        right = mid - 1;
      } else {
        // 如果findValue大於中間位置的值
        // 範圍在右半邊，不包含mid
        left = mid + 1;
      }
    }
    return rtnList;
  }
}
{% endhighlight %}