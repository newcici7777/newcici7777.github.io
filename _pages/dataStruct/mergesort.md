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
### 離開遞迴的條件
把陣列切割到只剩下1個元素，也就是left與right都是相同的，就離開遞迴，也就是return。
{% highlight java linenos %}
if (left == right) {
  return;
}
{% endhighlight %}

### left,right,mid
left是最左邊的索引0。
```
654321
↑
left
```

right是最右邊的索引5。
```
654321
     ↑
     right
```

mid中間值是(left + right) / 2 = (0 + 5) / 2<br>
整數相除，無條件捨去小數點，只留下整數，不會四捨五入。<br>
mid = 2<br>
```
654321
  ↑
 mid
```

## 左半邊切割
### 左半邊切割1
切割左半邊與右半邊。<br>
左半邊:left 到 mid ， 0 ~ 2<br>
右半邊:mid + 1 到 right ， 3 ~ 5<br>

```
654    321
  ↑    ↑
mid  mid+1
```

### 左半邊切割2
繼續切割<br>
mid中間值是(left + right) / 2 = (0 + 2) / 2 = 1<br>
```
654
 ↑
mid
```

切割左半邊與右半邊。<br>
左半邊:left 到 mid ， 0 ~ 1<br>
右半邊:mid + 1 到 right ， 2 ~ 2<br>

```
65   4
 ↑   ↑
mid  mid+1
```

### 左半邊切割3
left與right還不是同一個，還沒切割到一個元素，繼續切割。<br>
mid中間值是(left + right) / 2 = (0 + 1) / 2 = 0<br>
```
65
↑
mid
```
切割左半邊與右半邊。<br>
左半邊:left 到 mid ， 0 ~ 0<br>
右半邊:mid + 1 到 right ， 1 ~ 1<br>
```
6    5
↑    ↑
mid  mid+1
```

### 左半邊切割4-1
切到只剩下一個元素，離開目前方法，返回到切割3的方法。
```
6
```

### 左半邊切割4-2
切到只剩下一個元素，離開目前方法，返回到切割3的方法。
```
5
```

#### 切割過程
![img]({{site.imgurl}}/dataStruct/mergeSort1.jpg)  

## 左半邊合併
### 返回左半邊切割3
當切到只剩一個元素，就返回到切割前的方法，回到切割3的步驟。<br>
此時，left與mid都是0，mid+1與right都是1。<br>
```
6          5
↑          ↑
left,mid   mid+1,right
```
比較6與5看誰比較大，比較小的放入copyarr陣列。<br>
```
[5]
```

右半邊已經比完了，把左半邊全部放入copyarr陣列。
```
[5,6]
```

把copyarr的值拷貝到原本的陣列，目前left是0。<br>
原本的陣列:<br>
```
654321
```
把copyarr陣列`[5,6]`覆蓋到索引0與索引1的位置。
```
564321
```

### 返回左半邊切割2
因陣列的值已經在切割3修改，所以左半邊是56。
```
left right
↓    ↓
56   4
 ↑   ↑
mid  mid+1
```

比較左半邊的5與右半邊4看誰比較小，比較小的放入copyarr陣列。<br>
```
[4]
```

右半邊已經比完了，把左半邊全部放入copyarr陣列。
```
[4,5,6]
```

把copyarr的值拷貝到原本的陣列，目前left是0。<br>
原本的陣列:<br>
```
564321
```
把copyarr陣列`[4,5,6]`覆蓋到索引0到索引2的位置。
```
456321
```

此時左半邊都排好了，輪右半邊。


## 右半邊切割
### 右半邊切割1
切割左半邊與右半邊。<br>
左半邊:left 到 mid ， 0 ~ 2<br>
右半邊:mid + 1 到 right ， 3 ~ 5<br>

```
654    321
  ↑    ↑
mid  mid+1
```

### 右半邊切割2
left = 3, right = 5 <br>
繼續切割<br>
mid中間值是(left + right) / 2 = (3 + 5) / 2 = 4<br>
```
left=3
↓
321
 ↑
mid=4
```

切割左半邊與右半邊。<br>
左半邊:left 到 mid ， 3 ~ 4<br>
右半邊:mid + 1 到 right ， 5 ~ 5<br>

```
32   1
 ↑   ↑
mid  mid+1
```

### 右半邊切割3
left = 3, right = 4 <br>
left與right還不是同一個，還沒切割到一個元素，繼續切割。<br>
mid中間值是(left + right) / 2 = (3 + 4) / 2 = 3<br>
```
3 4
L R
↓ ↓
3 2
↑
mid
```
切割左半邊與右半邊。<br>
左半邊:left 到 mid ， 3 ~ 3<br>
右半邊:mid + 1 到 right ， 4 ~ 4<br>
```
3    2
↑    ↑
mid  mid+1
```

### 右半邊切割4-1
left = 3, right = 3 <br>
切到只剩下一個元素，離開目前方法，返回到切割3的方法。
```
3
```

### 右半邊切割4-2
left = 4, right = 4 <br>
切到只剩下一個元素，離開目前方法，返回到切割3的方法。
```
2
```

## 右半邊合併
### 返回右半邊切割3
當切到只剩一個元素，就返回到切割前的方法，回到切割3的步驟。<br>
此時，left與mid都是索引3，mid+1與right都是索引4。<br>
```
3          2
↑          ↑
left,mid   mid+1,right
```
比較3與2看誰比較小，比較小的放入copyarr陣列。<br>
```
[2]
```

右半邊已經比完了，把左半邊全部放入copyarr陣列。
```
[2,3]
```

把copyarr的值拷貝到原本的陣列，目前left是3。<br>
原本的陣列:<br>
```
456321
```
把copyarr陣列`[2,3]`覆蓋到索引3與索引4的位置。
```
456231
```

### 返回右半邊切割2
因陣列的值已經在切割3修改，所以右半邊是231。<br>
left索引是3，mid是4，mid+1是5，right索引是5。<br>
```
left 
↓    
23   1
 ↑   ↑
mid  mid+1
```

比較左半邊的left的值 = 2與右半邊mid+1的值 = 1看誰比較小，比較小的放入copyarr陣列。<br>
```
[1]
```

右半邊已經比完了，把左半邊全部放入copyarr陣列。
```
[1,2,3]
```

把copyarr的值拷貝到原本的陣列，目前left索引是3。<br>
原本的陣列:<br>
```
456231
```
把copyarr陣列`[1,2,3]`覆蓋到索引3到索引5的位置。
```
456123
```

## 左右邊合併
left是索引0，mid是索引2，mid+1是索引3，right是索引5。<br>
i變數放的是left索引，j變數放的是mid+1。<br>
i是左半邊的開頭，j是右半邊的開頭，mid是左半邊的結束，right是右半邊的結束。
```
i      j right
↓      ↓ ↓
456    123
  ↑    ↑
 mid  mid+1
```

比較左半邊的`arr[i] = 4`與右半邊`arr[j] = 1`看誰比較小，1比較小的放入copyarr陣列，j\+\+。<br>
```
[1]
```

```
i       j
↓       ↓
456    123
```

比較左半邊的`arr[i] = 4`與右半邊`arr[j] = 2`看誰比較小，2比較小的放入copyarr陣列，j\+\+。<br>
```
[1,2]
```

```
i        j
↓        ↓
456    123
```

比較左半邊的`arr[i] = 4`與右半邊`arr[j] = 3`看誰比較小，3比較小的放入copyarr陣列，j\+\+。<br>
```
[1,2,3]
```

右邊已經全比完了，左邊直接全部放進陣列中。
```
[1,2,3,4,5,6]
```

把copyarr的值拷貝到原本的陣列，目前left索引是0。<br>
原本的陣列:<br>
```
456123
```
把copyarr陣列`[1,2,3,4,5,6]`覆蓋到索引0到索引5的位置。
```
123456
```

## 合併程式碼解釋
{% highlight java linenos %}
int t = 0;  // 把比較大小的值放入臨時陣列copyarr，記錄copyarr的索引
int i = left;  // 左半邊的開頭
int j = mid + 1;  // 右半邊的開頭
// 左半邊的範圍left 到 mid， i設成左半邊的開頭left ，持續向後移動到 等於== mid
// 右半邊的範圍mid+1 到 right, j設成右半邊的開頭mid +1，持續向後移動到 等於== right
while (i <= mid && j <= right) {
  if (arr[i] < arr[j]) {
    copyarr[t++] = arr[i];
    i++;
  } else {
    copyarr[t++] = arr[j];
    j++;
  }
}
{% endhighlight %}

迴圈初始值
```
left
↓
i      j
↓      ↓
456    123
  ↑    ↑
 mid  mid+1
```

copyarr
```
 t = 0
 ↓
[ ]
```


迴圈第1次執行結束的結果如下:
```
i       j
↓       ↓
456    123
```
copyarr
```
   t = 1
   ↓
[1, ]
```

迴圈第2次執行結束的結果如下:
```
i        j
↓        ↓
456    123
```
copyarr
```
     t = 2
     ↓
[1,2, ]
```

迴圈第3次執行結束的結果如下:
```
i         j
↓         ↓
456    123
```
copyarr
```
       t = 3
       ↓
[1,2,3, ]
```

## 完整程式碼
{% highlight java linenos %}
public class MergeSort {
  public static void main(String[] args) {
    // 產生5個亂數
    int len = 5;
    int[] arr = new int[len];
    for (int i = 0; i < len; i++) {
      arr[i] = (int) (Math.random() * 100);
    }
    System.out.println("排序前:" + Arrays.toString(arr));
    
    // 可以放置排序好的值的臨時陣列
    int[] copyarr = new int[arr.length];
    // 切割
    cut(arr, 0, arr.length - 1, copyarr);
    System.out.println("排序後:" + Arrays.toString(arr));
  }

  public static void cut(int[] arr, int left, int right, int[] copyarr) {
    // 相同代表切割到只剩下一個
    if (left == right) {
      return;
    }
    // 取得中間值
    int mid = (left + right) / 2;
    // 切成左半邊(包含中間值)
    cut(arr, left, mid, copyarr);
    // 切成右半邊，中間值+1之後都是右半邊
    cut(arr, mid + 1, right, copyarr);
    // 合併
    merge(arr, left, right, mid, copyarr);
  }

  public static void merge(int[] arr, int left, int right, int mid, int[] copyarr) {
    int t = 0;  // 把比較大小的值放入臨時陣列copyarr，記錄copyarr的索引
    int i = left;  // 左半邊的開頭
    int j = mid + 1;  // 右半邊的開頭
    // 左半邊的範圍left 到 mid， i設成左半邊的開頭left ，持續向後移動到 等於== mid
    // 右半邊的範圍mid+1 到 right, j設成右半邊的開頭mid +1，持續向後移動到 等於== right
    while (i <= mid && j <= right) {
      // 若arr[i]小於 arr[j]
      if (arr[i] < arr[j]) {
        copyarr[t++] = arr[i];  // 把小的值複製到copyarr，t++
        i++;  // 向後移動
      } else {
        copyarr[t++] = arr[j];  // 把小的值複製到copyarr，t++
        j++;  // 向後移動
      }
    }

    // 如果左半邊已經全比完，把右半邊全部複製到copyarr
    while (j <= right) {
      copyarr[t++] = arr[j++];
    }

    // 如果右半邊已經全比完，把左半邊全部複製到copyarr
    while (i <= mid) {
      copyarr[t++] = arr[i++];
    }

    // left可能為0，也可能為右半邊的開頭
    // 把copyarr的已排序大小的值，覆蓋回原本陣列
    for (int k = 0; k < t; k++) {
      arr[left++] = copyarr[k];
    }
  }
}
{% endhighlight %}

[1]: {% link _pages/java/recursive.md %}
