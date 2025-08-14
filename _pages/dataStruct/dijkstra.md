---
title: Dijkstra
date: 2025-08-11
keywords: java, Dijkstra
---
## 事前準備
1. `visted[]`記錄「出發頂點」與下一次「距離最短的頂點」，「是否已訪問」的陣列。
2. `distance[]`記錄從「出發頂點」到每個頂點的最短距離。
3. `preVertex[]`記錄「前一個頂點」是誰，預設為全0。
4. MAX常數，代表無法連結，在這裡用10000。

## 出發頂點
### 出發頂點為0
![img]({{site.imgurl}}/java_datastruct/dijs1.png)

visted陣列，把出發頂點0設為True，預設全為False。<br>

頂點   |0|1|2|3|
visted|T|F|F|F|

把「出發頂點」到「出發頂點」的距離設為0，自己連自己沒有距離，其它頂點都用MAX。<br>

頂點     |0|1|2|3|
distance|0|MAX|MAX|MAX|

### 出發頂點0的相鄰頂點
出發頂點的相鄰頂點為1與2，記錄「出發頂點」→「相鄰頂點」的距離。<br>

![img]({{site.imgurl}}/java_datastruct/dijs2.png)

#### 出發頂點0到相鄰頂點1
出發頂點0 → 相鄰頂點1的距離為1，更新距離表。<br>
更新頂點1的前一個頂點為0，更新preVertex。<br>

頂點      |0|1|2|3|
distance |0|1|MAX|MAX|
preVertex| |0| | |

#### 出發頂點0到相鄰頂點2
出發頂點0 → 相鄰頂點2的距離為10。<br>
更新頂點2的前一個頂點為0，更新preVertex。<br>

distance|0|1|2|3|
distance|0|1|10|MAX|
preVertex| |0|0| |

## 距離最短的頂點1
在distance陣列中，找到最短距離的頂點，且沒有被訪問過。<br>
從以下表格中，找到的是1這個頂點，它與出發頂點的距離為1，為最短距離的頂點，把它設為已訪問。<br>

頂點     |0|1|2|3|
distance|0|1|10|MAX|
visted  |T|~~F~~ T|F|F|

![img]({{site.imgurl}}/java_datastruct/dijs3.png)

### 相鄰頂點2與3
距離最短的頂點1的相鄰頂點為0、2、3，但0已經被訪問過了`visted[0] = T`，所以沒被訪問過的相鄰頂點為2與3。<br>
![img]({{site.imgurl}}/java_datastruct/dijs4.png)

頂點     |0|1|2|3|
visted  |T|T|F|F|
distance|0|1|10|MAX|

### 0→1→2的距離
下圖中，「出發頂點0」到「最短距離頂點1」到「相鄰頂點2」的距離為1 \+ 2 = 3。<br>
0 → 1 = 1<br>
1 → 2 = 2<br>
0 → 1 → 2 = 1 \+ 2 = 3<br>

![img]({{site.imgurl}}/java_datastruct/dijs5.png)

原本的距離表出發頂點0到頂點2為10，3比10小，把距離表更新。<br>
更新頂點2的前一個頂點為1，因為從0→2跟0→1→2，0→1→2距離更短，前一個頂點是1，更新preVertex。<br>

頂點      |0|1|2|3|
visted   |T|T|F|F|
distance |0|1|~~10~~ 3|MAX|
preVertex| |0|~~0~~ 1| |

### 0→1→3的距離
下圖中，「出發頂點0」到「最短距離頂點1」到「相鄰頂點3」的距離為1 \+ 8 = 9。<br>

0 → 1 = 1<br>
1 → 3 = 8<br>
0 → 1 → 3 = 1 \+ 8 = 9 <br>

![img]({{site.imgurl}}/java_datastruct/dijs6.png)

原本的距離表出發頂點0到頂點3為MAX，9比MAX小，把距離表更新。<br>
更新頂點3的前一個頂點為1，因為從0→3跟0→1→3，0→1→3距離更短，前一個頂點是1，更新preVertex。<br>

頂點      |0|1|2|3|
visted   |T|T|F|F|
distance |0|1|3|~~MAX~~ 9|
preVertex| |0|1|1|

## 距離最短的頂點2
在distance陣列中，找到最短距離的頂點，且沒有被訪問過。<br>

![img]({{site.imgurl}}/java_datastruct/dijs7.png)

從以下表格中，找到的是2這個頂點，距離為3，把它設為已訪問。<br>

頂點      |0|1|2|3|
visted   |T|T|~~F~~ T|F|
distance |0|1|3|9|
preVertex| |0|1|1|

### 相鄰頂點3
頂點2的相鄰頂點為0、1、3，但0、1已經被訪問過了，所以沒被訪問過的相鄰頂點為3。<br>
![img]({{site.imgurl}}/java_datastruct/dijs8.png)

頂點      |0|1|2|3|
visted   |T|T|T|F|
distance |0|1|3|9|

根據上面的表格distance，「出發頂點0」到「最短距離頂點2」的最短路徑得到3。<br>
頂點2到「相鄰頂點3」的距離為3。<br>
0 → 2 = 3 <br>
2 → 3 = 3 <br>
0 → 2 → 3 = 3 + 3 = 6 <br>

![img]({{site.imgurl}}/java_datastruct/dijs9.png)

原本的距離表出發頂點0到頂點3為9，6比9小，距離表更新。<br>
更新頂點3的前一個頂點為2，因為0→1→3 = 9，而0→2→3 = 6，前一個頂點為2的路徑更短，更新preVertex。<br>

頂點      |0|1|2|3|
visted   |T|T|T|F|
distance |0|1|3|~~9~~ 6|
preVertex| |0|1|~~1~~ 2|

## 距離最短的頂點3
在distance陣列中，找到最短距離的頂點，且沒有被訪問過的為頂點3，設為已訪問。<br>

頂點      |0|1|2|3|
visted   |T|T|T|~~F~~ T|
distance |0|1|3|6|
preVertex| |0|1|2|

![img]({{site.imgurl}}/java_datastruct/dijs10.png)<br>

因為其它頂點全部都訪問過了，求出頂點0到各個頂點最短距離如下:<br>

頂點      |0|1|2|3|
distance |0|1|3|6|

假設要尋找頂點0→頂點2的最短路徑，參考下表，頂點2的前一個頂點1，1的前一個頂點是0，最短路徑為2→1→0。<br>

頂點      |0|1|2|3|
preVertex| |0|1|2|

{% highlight java linenos %}
public class Dijkstra {
  private int[][] matrix ;
  private boolean[] visted;
  private int[] preVertex;
  private int[] distance;
  private int vertexLen = 4;
  public static final int MAX = 10000;
  public Dijkstra() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{MAX, 1, 10, MAX};
    matrix[1] = new int[]{1, MAX, 2, 8};
    matrix[2] = new int[]{10, 2, MAX, 3};
    matrix[3] = new int[]{MAX, 8, 3, MAX};

    visted = new boolean[vertexLen];
    preVertex = new int[vertexLen];
    distance = new int[vertexLen];
    Arrays.fill(distance, MAX);
  }
  public void setStart(int startIndex) {
    distance[startIndex] = 0;
    visted[startIndex] = true;
  }
  public void dijkstra(int index) {
    int len = 0;
    for (int i = 0; i < matrix[index].length; i++) {
      len = distance[index] + matrix[index][i];
      if (!visted[i] && len < distance[i]) {
        distance[i] = len;
        preVertex[i] = index;
      }
    }
  }

  public static void main(String[] args) {
    Dijkstra dijkstra = new Dijkstra();
    dijkstra.setStart(0);
    dijkstra.dijkstra(0);
    System.out.println(Arrays.toString(dijkstra.distance));
    for (int i = 0; i < dijkstra.vertexLen - 1; i++) {
      int vertex = dijkstra.findMinPathVertex();
      System.out.println("vertex = " + vertex);
      dijkstra.dijkstra(vertex);
      System.out.println(Arrays.toString(dijkstra.distance));
      System.out.println(Arrays.toString(dijkstra.preVertex));
      System.out.println("============");
    }
  }

  public int findMinPathVertex() {
    int min = MAX;
    int vertex = 0;
    for (int i = 0; i < vertexLen; i++) {
      if (!visted[i] && distance[i] < min) {
        min = distance[i];
        vertex = i;
      }
    }
    visted[vertex] = true;
    return vertex;
  }
}
{% endhighlight %}