---
title: Bucket Sort
date: 2025-08-06
keywords: java, Bucket Sort
---
## 想法與步驟
### 籃子大小
把數字分到10個籃子，每一個籃子代表0到9的數字。<br>
但每個籃子的深度不太確定，使用10，代表每一次使用籃子，最多只能放10個數字。<br>

### 個位十位百位
第1次求出個位數，把數字mod10，得到餘數，把餘數放在對映數字的籃子中。<br>
第2次求出十位數，把數字除10，取出商數，商數mod10，得到餘數，把餘數放在對映數字的籃子中。<br>
第3次求出百位數，把數字除100，取出商數，商數mod10，得到餘數，把餘數放在對映數字的籃子中。<br>

### 步驟如下
1. 準備大小10 \* 10的二維陣列。`bucket[10][10]`
2. 準備大小10的一維陣列，存放每個0-9籃子中，記錄數字的數量。`bucketCnt[10]`
3. 第1次，求出陣列中值的「個位數」，mod10，把餘數放在對映數字的籃子中。
4. 再把籃子中的數字，按照順序，存放回原本陣列。
5. 第2次，求出陣列中值的「十位數」，mod10，把餘數放在對映數字的籃子中。
6. 再把籃子中的數字，按照順序，存放回原本陣列。
7. 第3次，求出陣列中值的「百位數」，mod10，把餘數放在對映數字的籃子中。
8. 再把籃子中的數字，按照順序，存放回原本陣列。

## 步驟圖
### 個位數
陣列的值 mod 10，把餘數放在對映數字的籃子中。<br>
再把籃子中的數字，按照順序，存放回原本陣列。<br>
![img]({{site.imgurl}}/java_datastruct/bucket1.png)

### 十位數
十位數請看綠色數字。<br>
陣列的值先除10，求出十位數，十位數再mod 10，把餘數放在對映數字的籃子中。<br>
例如:333的十位數是333 / 10 = 33。<br>
33 mod 10 = 3。<br>
放到3的籃子中。<br>
再把籃子中的數字，按照順序，存放回原本陣列。<br>
![img]({{site.imgurl}}/java_datastruct/bucket10.png)

### 百位數
百位數請看紅色數字。<br>
陣列的值先除100，求出百位數，百位數再mod 10，把餘數放在對映數字的籃子中。<br>
例如:333的百位數是333 / 100 = 3。<br>
3 mod 10 = 3。<br>
放到3的籃子中。<br>
再把籃子中的數字，按照順序，存放回原本陣列。<br>
![img]({{site.imgurl}}/java_datastruct/bucket100.png)


{% highlight java linenos %}
public class bucketsort {
  public static void main(String[] args) {
    int[] arr = {53, 3, 542, 748, 14, 214};
    int len = 10;
    int[][] bucket = new int [len][len];
    int[] bucketCnt = new int[len];

    // 個位
    for (int j = 0; j < arr.length; j++) {
      // 求出個位 mod10 的餘數
      int digit = arr[j] % 10;
      // 把餘數放入對映的籃子
      // bucketCnt是記錄每個籃子的數量
      // 假如digit餘數是8，第1次bucketCnt[8] = 0
      // 下面就會變成
      // bucket[8][0] = 888
      bucket[digit][bucketCnt[digit]] = arr[j];
      // bucketCnt[8]++，裡面的值由0變成1
      // 記錄每個數字的數量。
      bucketCnt[digit]++;
    }
    // 把籃子中的數字複製回原本的陣列
    int index = 0;  // 陣列索引
    // len代表10個籃子
    for (int k = 0; k < len; k++) {
      // 記錄籃子的數量不為0，代表這個籃子有數字
      if (bucketCnt[k] != 0) {
        // bucketCnt[k]籃子的數量
        for (int m = 0; m < bucketCnt[k]; m++) {
          // 籃子中的數字指派給原本陣列
          arr[index++] = bucket[k][m];
        }
        // bucketCnt[k]籃子的數量要變回0
        // 不然下一次十位數，籃子數量不為0，會影嚮每個籃子數量記錄
        bucketCnt[k] = 0;
      }
    }
    System.out.println(Arrays.toString(arr));
    
    // 十位
    for (int j = 0; j < arr.length; j++) {
      int digit = arr[j] / 10 % 10;
      bucket[digit][bucketCnt[digit]] = arr[j];
      bucketCnt[digit]++;
    }
    index = 0;
    for (int k = 0; k < len; k++) {
      if (bucketCnt[k] != 0) {
        for (int m = 0; m < bucketCnt[k]; m++) {
          arr[index++] = bucket[k][m];
        }
        bucketCnt[k] = 0;
      }
    }
    System.out.println(Arrays.toString(arr));

    // 百位
    for (int j = 0; j < arr.length; j++) {
      int digit = arr[j] / 100 % 10;
      bucket[digit][bucketCnt[digit]] = arr[j];
      bucketCnt[digit]++;
    }
    index = 0;
    for (int k = 0; k < len; k++) {
      if (bucketCnt[k] != 0) {
        for (int m = 0; m < bucketCnt[k]; m++) {
          arr[index++] = bucket[k][m];
        }
        bucketCnt[k] = 0;
      }
    }
    System.out.println(Arrays.toString(arr));
  }
}
{% endhighlight %}

把以上個位數、十位數、百位數，重新整合。
{% highlight java linenos %}
public class BucketSort2 {
  public static void main(String[] args) {
    int[] arr = {333, 555, 13, 25, 3, 5};
    int len = 10; // 10 個籃子
    int[][] bucket = new int [len][len];
    int[] bucketCnt = new int[len];
    // 求出陣列中最大的數字
    int max = arr[0];
    for (int i = 1; i < arr.length; i++) {
      if (arr[i] > max) {
        max = arr[i];
      }
    }
    // 求出最大數字的字母個數，但333的字母個數是3，5555字母個數為4。
    int letterCnt = (max + "").length();
    System.out.println(letterCnt);
    // div為除數，第1次是1，第2次是10個位數，第3次是100百位數，第4次是1000千位數
    for (int i = 1, div = 1; i <= letterCnt; i++, div*= 10) {
      for (int j = 0; j < arr.length; j++) {
        // 求出個位、十位、百位 mod10的餘數
        int digit = (arr[j] / div) % 10;
        // 把餘數放入對映的籃子
        // bucketCnt是記錄每個籃子的數量
        // 假如digit餘數是8，第1次bucketCnt[8] = 0
        // 下面就會變成
        // bucket[8][0] = 888
        bucket[digit][bucketCnt[digit]] = arr[j];
        // bucketCnt[8]++，裡面的值由0變成1
        // 記錄每個數字的數量。
        bucketCnt[digit]++;
      }
      // 把籃子中的數字複製回原本的陣列
      int index = 0;  // 陣列索引
      // len代表10個籃子
      for (int k = 0; k < len; k++) {
        // 記錄籃子的數量不為0，代表這個籃子有數字
        if (bucketCnt[k] != 0) {
          // bucketCnt[k]籃子的數量
          for (int m = 0; m < bucketCnt[k]; m++) {
            // 籃子中的數字指派給原本陣列
            arr[index++] = bucket[k][m];
          }
          // bucketCnt[k]籃子的數量要變回0
          // 不然下一次十位數，籃子數量不為0，會影嚮每個籃子數量記錄
          bucketCnt[k] = 0;
        }
      }
      System.out.println(Arrays.toString(arr));
    }
  }
}
{% endhighlight %}
