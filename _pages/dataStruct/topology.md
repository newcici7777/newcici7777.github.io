---
title: 拓樸排序
date: 2025-08-17
keywords: java, Topology Sort
---
拓樸排序，是根據分支度小的先排序。

所謂的分支度，在無向圖中，是一個頂點連接的邊的總數。

下圖中，頂點0有1條邊連著頂點1，頂點1有2條邊連著頂點0與頂點2，頂點2有1條邊連著頂點1，這連著的邊就是分支度。<br>

![img]({{site.imgurl}}/java_datastruct/topo1.png)<br>

拓樸排序，是由分支度最小的頂點開始排序。

## 無向圖
所謂的分支度，在無向圖中是多少條邊edge連到頂點的數量。<br>

下圖中，頂點0有1條邊連著，頂點1有2條邊連著，頂點2有1條邊連著，這連著的邊就是分支度。<br>

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

### 準備
1.準備degree陣列，記錄每個頂點的分支度(連著頂點的邊)的數量，大小為頂點的數量。<br>

|頂點  |0|1|2|
|分支度|1|2|1|

### 步驟
分支度小的頂點先從圖中把邊刪掉，其它跟它相連的頂點，也要把他們的分支度減1，直到所有頂點都訪問完畢，代表遍歷完成。

1.接下來把所有degree等於1的頂點放入Queue。<br>
degree等於1，代表只有一條邊連著，由最少的邊開始。<br>

把degree為1的頂點0與2放入Queue中。<br>

![img]({{site.imgurl}}/java_datastruct/topo_q1.png)<br>

2.把Queue 中的元素拿出來，首先拿到的是0(先入先出)。<br>

![img]({{site.imgurl}}/java_datastruct/topo_q2.png)<br>

3.把頂點0的一條邊刪掉，頂點0要從圖上消失。<br>

![img]({{site.imgurl}}/java_datastruct/topo2.png)<br>

並輸出在螢幕。<br>
```
0,
```

4.因為頂點0已被刪掉，跟頂點0有連接的鄰居們，鄰居跟頂點0有連線的邊也要刪掉1條。<br>

|頂點  |0|1|2|
|分支度|1|~~2~~ <span class="markline">1</span>|1|

頂點1刪掉的<span class="markline">degree剩下1</span>，把它加入Queue。<br>

![img]({{site.imgurl}}/java_datastruct/topo_q3.png)<br>

5.把Queue中的頂點2拿出來，頂點2要從圖上消失。<br>

![img]({{site.imgurl}}/java_datastruct/topo_q4.png)<br>

![img]({{site.imgurl}}/java_datastruct/topo3.png)<br>

並輸出在螢幕。<br>
```
0, 2
```

6.因為頂點2已被刪掉，跟頂點2有連接的鄰居們，鄰居跟頂點2有連線的邊也要刪掉1條。<br>

|頂點  |0|1|2|
|分支度|1|~~1~~ <span class="markline">0</span>|1|

7.把Queue中的元素1拿出來。<br>

![img]({{site.imgurl}}/java_datastruct/topo_q5.png)<br>

並輸出在螢幕。<br>
```
0, 2, 1
```

8.Queue已經沒有元素，拓樸排序遍歷完成。

### 程式碼
{% highlight java linenos %}
public class Topology {
  // 相鄰矩陣
  private int[][] matrix;
  // 頂點數量
  private int vertexLen = 3;
  // 分支度
  private int[] degree;

  public int getVertexLen() {
    return vertexLen;
  }

  public Topology() {
    matrix = new int[vertexLen][vertexLen];
    // 1代表相鄰
    matrix[0] = new int[]{0, 1, 0};
    matrix[1] = new int[]{1, 0, 1};
    matrix[2] = new int[]{0, 1, 0};
    degree = new int[vertexLen];
    for (int i = 0; i < vertexLen; i++) {
      for (int j = 0; j < vertexLen; j++) {
        if (matrix[i][j] == 1) {
          // 以列(row)的方式統計分支度
          degree[i]++;
        }
      }
    }
  }

  public int topology() {
    LinkedList<Integer> queue = new LinkedList<Integer>();
    // 訪問的頂點數量
    int vistedCnt = 0;
    for (int i = 0; i < degree.length; i++) {
      // degree == 1就加入queue
      if (degree[i] == 1) {
        queue.add(i);
        // 每加一次queue，訪問的頂點數量+1
        vistedCnt++;
      }
    }
    while (!queue.isEmpty()) {
      // 從queue取出頂點，代表此頂點要消失
      int vertex = queue.poll();
      // 印出
      System.out.print(vertex + ",");
      // 檢查是否有相鄰的頂點
      for (int j = 0; j < vertexLen; j++) {
        // 有相鄰的頂點j，分支度要減1
        if (matrix[vertex][j] == 1) {
          degree[j]--;
          // degree == 1就加入queue
          if (degree[j] == 1) {
            queue.add(j);
            // 每加一次queue，訪問的頂點數量+1
            vistedCnt++;
          }
        }
      }
    }
    System.out.println();
    return vistedCnt;
  }

  public static void main(String[] args) {
    Topology topology = new Topology();
    int visitedCnt = topology.topology();
    // 若訪問的頂點數量跟實際的頂點數量一樣，代表沒有迴路
    System.out.println("hasCircle ? " + (visitedCnt != topology.vertexLen));
  }
}
{% endhighlight %}
```
0,2,1,
hasCircle ? false
```

## 判斷無向圖是否有迴路
下圖中，頂點0有2條邊連著，頂點1有2條邊連著，頂點2有2條邊連著，這連著的邊就是分支度。<br>
![img]({{site.imgurl}}/java_datastruct/topo_circle1.png)<br>

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

如果要判斷無向圖，是否「有迴路」，除了union find可判斷是否有迴路，拓撲排序也可以判斷圖中是否有迴路。

### 判斷迴路
1.準備degree陣列，記錄每個頂點的分支度，大小為頂點的數量。<br>

|頂點  |0|1|2|
|分支度|2|2|2|

3.發現根本沒有頂點的分支度是1，所以沒有一個頂點加到Queue中。<br>

4.有加到Queue的頂點數量跟實際的頂點數量不符，代表有迴路。<br>

直接把先前的程式中的相鄰矩陣改成有迴路。
{% highlight java linenos %}
matrix = new int[vertexLen][vertexLen];
matrix[0] = new int[]{0, 1, 1};
matrix[1] = new int[]{1, 0, 1};
matrix[2] = new int[]{1, 1, 0};
{% endhighlight %}

## 有向圖
有向圖中，只在乎「入」分支度。什麼是入分支度？就是有幾個箭頭指向頂點。<br>

![img]({{site.imgurl}}/java_datastruct/topo_in1.png)<br>

### 入分支度
i代表從i頂點，箭頭指向j。<br>
i代表「出」邊，j代表「入」邊。<br>
所以入分支度的統計，以column欄，箭頭指向的j為主，垂直統計有多少頂點指向自己。

| |j=0|j=1|j=2|j=3|
|i=0|0|1|0|0|
|i=1|0|0|1|0|
|i=2|0|0|0|0|
|i=3|0|1|0|0|
|入分支度|0|2|1|0|

|頂點   |0|1|2|3|
|入分支度|0|2|1|0|

與先前程式碼大同小異，主要是相鄰矩陣入分支度的統計。

入分支度為0(代表沒有頂點指向自己)，加入Queue，從最少的度開始。

{% highlight java linenos %}
public class InDegreeTopology {
  // 相鄰矩陣
  private int[][] matrix;
  // 頂點數量
  private int vertexLen = 4;
  // 入分支度
  private int[] indegree;

  public int getVertexLen() {
    return vertexLen;
  }

  public InDegreeTopology() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{0, 1, 0, 0};
    matrix[1] = new int[]{0, 0, 1, 0};
    matrix[2] = new int[]{0, 0, 0, 0};
    matrix[3] = new int[]{0, 1, 0, 0};
    indegree = new int[vertexLen];
    for (int i = 0; i < vertexLen; i++) {
      for (int j = 0; j < vertexLen; j++) {
        if (matrix[i][j] == 1) {
          // 入分支度以j為主，有多少箭頭指向此頂點
          indegree[j]++;
        }
      }
    }
    //System.out.println(Arrays.toString(indegree));
  }

  public int topology() {
    LinkedList<Integer> queue = new LinkedList<>();
    // 計算有加入queue中的頂點數量
    int vistedCnt = 0;
    for (int i = 0; i < indegree.length; i++) {
      // 入分支度為0，代表沒有頂點指向
      if (indegree[i] == 0) {
        // 加入queue
        queue.add(i);
        // 計算有加入queue中的頂點數量 +1
        vistedCnt++;
      }
    }
    // 迴圈進入的條件，queue不為空
    while (!queue.isEmpty()) {
      // 把頂點從圖上消失
      int vertex = queue.poll();
      // 印出頂點
      System.out.print(vertex + ", ");
      // 檢查是否有出邊，也就是箭頭指向其它頂點
      for (int j = 0; j < vertexLen; j++) {
        // 箭頭指向其它頂點
        if (matrix[vertex][j] == 1) {
          // 把那個頂點入分支度減1
          indegree[j]--;
          // 若那個頂點入分支度為0，代表沒人指向它
          if (indegree[j] == 0) {
            // 加入queue
            queue.add(j);
            // 計算有加入queue中的頂點數量 +1
            vistedCnt++;
          }
        }
      }
    }
    System.out.println();
    return vistedCnt;
  }

  public static void main(String[] args) {
    InDegreeTopology inDegreeTopology = new InDegreeTopology();
    int vistedCnt = inDegreeTopology.topology();
    // 若訪問的頂點數量跟實際的頂點數量一樣，代表沒有迴路
    System.out.println("hasCircle = " + (vistedCnt != inDegreeTopology.vertexLen));
  }
}
{% endhighlight %}
```
0, 3, 1, 2, 
hasCircle = false
```