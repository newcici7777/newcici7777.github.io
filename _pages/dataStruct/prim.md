---
title: Prim
date: 2025-08-27
keywords: java, Prim
---
## 理論
Prim是尋找從某頂點出發，到全部頂點的最短路徑。<br>

最短路徑(邊)的數量是頂點的數量 - 1，才能是到達各頂點之間的最少路徑(邊)。<br>

3個頂點最少連接的邊是2條。(頂點數量3 - 1 = 最少連接的邊數2)<br>
![img]({{site.imgurl}}/java_datastruct/mst1.png)<br>

4個頂點最少連接的邊是3條。(頂點數量4 - 1 = 最少連接的邊數3)<br>
![img]({{site.imgurl}}/java_datastruct/mst2.png)<br>

從一個圖中，找出最少的邊數，但可以連接所有頂點。<br>
![img]({{site.imgurl}}/java_datastruct/mst3.png)<br>

樹，是沒有迴路的，上圖中的紅色的線連接所有頂點，可以把它看成一棵樹。<br>

要在圖Graph中找一棵樹，這一棵樹中文是最小成本展開樹Minimum Cost Spanning Tree，縮寫為MST，可以想像為要在圖中找最短路徑可以連接到各個頂點的樹，最短路徑也就是最小的成本。<br>

Prim著重在<span class="markline">頂點</span>，由某個頂點出發，檢查此頂點出發的邊，那個最小。<br>

## 找出最短路徑的邊與頂點
找出最短路徑的邊，尋找次數為(頂點的數量 - 1)。
### 第1次
把頂點0加入到訪問清單，從頂點0出發，有2條路，那一條更短？<br>
![img]({{site.imgurl}}/java_datastruct/prim1.png)<br>

頂點0 到 頂點1的路更短，把「頂點1」加入訪問清單。<br>
頂點0已經在訪問清單中了，所以不再加入。<br>
![img]({{site.imgurl}}/java_datastruct/prim2.png)<br>

### 第2次
訪問清單目前有頂點0與頂點1。

頂點0:尋找跟頂點0連著的邊誰最短，排除掉已訪問過的邊。<br>
![img]({{site.imgurl}}/java_datastruct/prim3.png)<br>

頂點1:尋找跟頂點1連著的邊誰最短，排除掉已訪問過的邊。<br>
![img]({{site.imgurl}}/java_datastruct/prim4.png)<br>

以上結果，誰的邊最短？<br>
![img]({{site.imgurl}}/java_datastruct/prim4.png)<br>
頂點1 到 頂點2的路更短，最短路徑相連的「頂點2」，加入訪問清單。<br>
頂點1已經在訪問清單中了，所以不再加入。<br>

### 第3次
訪問清單目前有頂點0與頂點1與頂點2。

頂點0:尋找跟頂點0連著的邊誰最短，排除掉已訪問過的邊。<br>
![img]({{site.imgurl}}/java_datastruct/prim3.png)<br>

頂點1:尋找跟頂點1連著的邊誰最短，排除掉已訪問過的邊。<br>
![img]({{site.imgurl}}/java_datastruct/prim5.png)<br>

頂點2:尋找跟頂點1連著的邊誰最短，排除掉已訪問過的邊。<br>
![img]({{site.imgurl}}/java_datastruct/prim6.png)<br>

以上結果，誰的邊最短？<br>
![img]({{site.imgurl}}/java_datastruct/prim6.png)<br>
頂點2 到 頂點3的路更短，最短路徑相連的「頂點3」，加入訪問清單。<br>
頂點2已經在訪問清單中了，所以不再加入。<br>

## 準備工作
Prim演算法，要先把相鄰矩陣中，不能連接的邊全設為最大值MAX，包含自己連自己也要設為MAX。<br>

{% highlight java linenos %}
matrix = new int[vertexLen][vertexLen];
matrix[0] = new int[]{MAX, 1, 10, MAX};
matrix[1] = new int[]{1, MAX, 2, 8};
matrix[2] = new int[]{10, 2, MAX, 3};
matrix[3] = new int[]{MAX, 8, 3, MAX};
{% endhighlight %}

visted陣列，記錄每一輪最短距離相連的頂點。

|頂點   |0|1|2|3|
|visted|F|F|F|F|

## 程式碼
{% highlight java linenos %}
public class Prim {
  private int[][] matrix;
  private int vertexLen = 4;
  public static final int MAX = 10000;
  // 訪問清單
  private boolean[] visted;

  public Prim() {
    // 不能連接的邊全設為最大值MAX，包含自己連自己也要設為MAX
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{MAX, 1, 10, MAX};
    matrix[1] = new int[]{1, MAX, 2, 8};
    matrix[2] = new int[]{10, 2, MAX, 3};
    matrix[3] = new int[]{MAX, 8, 3, MAX};
    visted = new boolean[vertexLen];
  }

  public void findShortDist() {
    // 把頂點0加入訪問清單
    visted[0] = true;
    // 儲存最短路徑，也就最短的邊
    List<int[]> result = new ArrayList<>();
    // 尋找最短路徑次數為頂點數量 - 1
    for (int i = 0; i < vertexLen - 1; i++) {
      findShortDistVertex(result);
    }
    // 印出最短路徑，也就是最短的邊
    for (int[] arr : result) {
      System.out.println(Arrays.toString(arr));
    }
  }

  public void findShortDistVertex(List<int[]> result) {
    // 把最小權重(距離)設為最大值MAX
    int minWeight = MAX;
    // 最短距離連接的頂點，預設-1
    int x = -1;
    // 最短距離連接的頂點，預設-1
    int y = -1;
    // 遍歷訪問清單(若為最短距離頂點，會被加入清單中)
    for (int i = 0; i < visted.length; i++) {
      // 頂點是否在清單中
      if (visted[i]) {
      	// 相連的邊那一條最短，而且相連的頂點未被訪問過
        for (int j = 0; j < vertexLen; j++) {
          // matrix[i][j]不為 MAX，代表頂點i與頂點j是有相連
          if (matrix[i][j] != MAX && !visted[j]) {
          	// 距離是否比最小距離小
            if (matrix[i][j] < minWeight) {
              // 替換成最小距離
              minWeight = matrix[i][j];
              // 記錄最小距離相連的頂點
              x = i;
              y = j;
            }
          }
        }
      }
    }
    System.out.println("x = " + x + ", y = " + y + ", weight = " + minWeight);
    // 把最小距離的頂點y加入清單
    // x頂點已在清單中，不用再加入
    visted[y] = true;
    // 儲存最短距離的邊
    result.add(new int[]{x, y});
  }

  public static void main(String[] args) {
    Prim prim = new Prim();
    prim.findShortDist();
  }
}
{% endhighlight %}
```
x = 0, y = 1, weight = 1
x = 1, y = 2, weight = 2
x = 2, y = 3, weight = 3
[0, 1]
[1, 2]
[2, 3]
```