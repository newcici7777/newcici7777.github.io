---
title: 拓撲排序
date: 2025-08-17
keywords: java, Topology Sort
---
所謂的拓撲排序中的「排序」，並非是由小到大或由大到小。而是誰先誰後，是先後的問題。<br>
在了解拓撲排序之前，要先了解分支度的概念。<br>

## 無向圖
所謂的分支度，在無向圖中是多少條邊edge連到頂點的數量。<br>
下圖中，頂點0有2條邊連著，頂點1有2條邊連著，頂點2有2條邊連著，這此連著的邊就是分支度。<br>
![img]({{site.imgurl}}/java_datastruct/topo1.png)<br>

### 分支度
分支度的統計可以列(row)的方式統計。

| |j=0|j=1|j=2|分支度|
|i=0|0|1|0|1|
|i=1|1|0|1|2|
|i=2|0|1|0|1|

分支度的統計可以欄(column)的方式統計。

| |j=0|j=1|j=2|
|i=0|0|1|0|
|i=1|1|0|1|
|i=2|0|1|0|
|分支度|1|2|1|

### 拓撲排序步驟
1.準備degree陣列，記錄每個頂點的分支度(連著頂點的邊)的數量，大小為頂點的數量。<br>

|頂點  |0|1|2|
|分支度|1|2|1|

2.準備是否已訪問過的陣列，避免重覆訪問，大小為頂點的數量。<br>

|頂點   |0|1|2|
|visted|F|F|F|

3.接下來把所有degree等於1的頂點放入Queue，並設為已訪問。<br>
degree等於1，代表只有一條邊連著。<br>

Queue
```
出 ← 0,2  ← 入
```

|頂點   |0|1|2|
|visted|~~F~~T|F|~~F~~T|

4.把Queue 中的元素拿出來，首先拿到的是0(先入先出)。<br>

Queue剩下的元素
```
出 ← 2  ← 入
```

5.把頂點0的degree減1，意思是把頂點0的一條邊刪掉，頂點0要從圖上分離出來。<br>

|頂點  |0|1|2|
|分支度|~~1~~ 0|2|1|

![img]({{site.imgurl}}/java_datastruct/topo2.png)<br>

6.找找看之前跟頂點0有連接的鄰居，並且沒有被訪問過，把它們跟頂點0有連線的邊也刪掉1條。<br>
頂點0之前連接的鄰居是頂點1。<br>

|頂點  |0|1|2|
|分支度|0|~~2~~ 1|1|

把頂點1設為已訪問。<br>

|頂點   |0|1|2|
|visted|T|~~F~~ T|T|

頂點1刪掉的degree只剩下1，把它加入Queue。<br>

Queue的元素
```
出 ← 2, 1  ← 入
```

7.把Queue中的元素2拿出來，一樣把degree減1。<br>

Queue剩下的元素
```
出 ← 1  ← 入
```

|頂點  |0|1|2|
|分支度|0|1|~~1~~ 0|

![img]({{site.imgurl}}/java_datastruct/topo3.png)<br>

8.找找看之前跟頂點0有連接的鄰居，並且沒有被訪問過。<br>
目前所有頂點都已經訪問過了。<br>

9.把Queue中的元素1拿出來，一樣把degree減1。<br>

|頂點  |0|1|2|
|分支度|0|~~1~~ 0|0|

10.Queue已經沒有元素，拓樸排序遍歷完成。

### 分支度
分支度的統計可以列(row)的方式統計。

| |j=0|j=1|j=2|分支度|
|i=0|0|1|1|2|
|i=1|1|0|1|2|
|i=2|1|1|0|2|

分支度的統計可以欄(column)的方式統計。

| |j=0|j=1|j=2|
|i=0|0|1|1|
|i=1|1|0|1|
|i=2|1|1|0|
|分支度|2|2|2|

如果要判斷無向圖，是否「有環」，除了union find可判斷是否有環，拓撲排序也可以判斷圖中是否有環。

### 判斷環
1.準備degree陣列，記錄每個頂點的分支度，大小為頂點的數量。<br>

|頂點  |0|1|2|
|分支度|2|2|2|

2.準備是否已訪問過的陣列，避免重覆訪問，大小為頂點的數量。<br>

|頂點   |0|1|2|
|visted|F|F|F|

3.cnt變數，記錄當degree為1，且沒有被訪問過的頂點，當訪問完所有頂點，cnt的數量若與頂點數量相同，代表沒有環。<br>

{% highlight java linenos %}
import java.util.Arrays;
import java.util.LinkedList;

public class Topology {
  private int[][] matrix;
  private int vertexLen = 3;
  private int[] degree;
  private boolean[] visted;

  public Topology() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{0, 1, 0};
    matrix[1] = new int[]{1, 0, 1};
    matrix[2] = new int[]{0, 1, 0};
    degree = new int[vertexLen];
    visted = new boolean[vertexLen];
    for (int i = 0; i < vertexLen; i++) {
      for (int j = 0; j < vertexLen; j++) {
        if (matrix[i][j] == 1) {
          degree[i]++;
        }
      }
    }
    System.out.println(Arrays.toString(degree));
  }

  public boolean checkCircle() {
    LinkedList<Integer> queue = new LinkedList<Integer>();
    int cnt = 0;
    for (int i = 0; i < degree.length; i++) {
      if (!visted[i] && degree[i] == 1) {
        queue.add(i);
        visted[i] = true;
        cnt++;
      }
    }
    while (!queue.isEmpty()) {
      int vertex = queue.poll();
      degree[vertex]--;
      for (int j = 0; j < vertexLen; j++) {
        if (!visted[j] && matrix[vertex][j] == 1) {
          degree[j]--;
          if (degree[j] == 1) {
            queue.add(j);
            visted[j] = true;
            cnt++;
          }
        }
      }
    }
    return cnt != vertexLen;
  }

  public static void main(String[] args) {
    Topology topology = new Topology();
    boolean hasCircle = topology.checkCircle();
    System.out.println(hasCircle);
  }
}
{% endhighlight %}


## 有向圖
### 出分支度
### 入分支度