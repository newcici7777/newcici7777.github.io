---
title: Kruskal
date: 2025-08-16
keywords: java, KrusKal
---
Prerequisites:

- [union find][1]

請一定要了解[union find][1]，才能繼續往下閱讀。<br>

KrusKal的步驟如下:<br>
1. 把所有連接的邊，按照權值，由小到大排序。
2. 檢查邊是否有環，沒有環就加入到list中。
3. list記錄的是圖中每個頂點相連的最小路徑。

{% highlight java linenos %}
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Krus {
  private int[][] matrix ;
  private int vertexLen = 4;
  public static final int MAX = 10000;
  private int[] parent;
  public Krus() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{0, 1, 10, MAX};
    matrix[1] = new int[]{1, 0, 2, 8};
    matrix[2] = new int[]{10, 2, 0, 3};
    matrix[3] = new int[]{MAX, 8, 3, 0};
    // 以下是union find演算法
    parent = new int[vertexLen];
    for (int i = 0; i < vertexLen; i++) {
      parent[i] = i;
    }
  }
  public EdgeData[] getEdges() {
    int edgeNum = 0;
    for (int i = 0; i < matrix.length; i++) {
      // 只看對角線0的右邊。
      for (int j = i + 1; j < matrix.length; j++) {
        // MAX代表不是相連的
        if (matrix[i][j] != MAX) {
          // 計算邊的數量
          edgeNum++;
        }
      }
    }
    // 建立邊的陣列
    EdgeData[] edges = new EdgeData[edgeNum];
    // 邊陣列的索引
    int index = 0;
    for (int i = 0; i < matrix.length; i++) {
      // 只看對角線0的右邊。
      for (int j = i + 1; j < matrix.length; j++) {
        // MAX代表不是相連的
        if (matrix[i][j] != MAX) {
          // 建立邊
          EdgeData edgeData = new EdgeData();
          edgeData.setStart(i);
          edgeData.setEnd(j);
          edgeData.setWeight(matrix[i][j]);
          // 把邊放入陣列
          edges[index++] = edgeData;
        }
      }
    }
    return edges;
  }
  // 把邊按照權重，由小到大排序，採用氣泡排序
  public void sort(EdgeData[] edges) {
    for (int i = 0; i < edges.length; i++) {
      for (int j = 0; j < edges.length - 1 - i; j++) {
        if (edges[j].getWeight() > edges[j + 1].getWeight()) {
          EdgeData temp = edges[j];
          edges[j] = edges[j + 1];
          edges[j + 1] = temp;
        }
      }
    }
  }

  public int find(int vertex) {
    if (parent[vertex] == vertex) {
      return vertex;
    }
    return find(parent[vertex]);
  }

  public void union(int vertex1, int vertex2) {
    int parent1 = find(vertex1);
    int parent2 = find(vertex2);
    if (parent1 != parent2) {
      parent[parent1] = parent2;
    }
  }

  public static void main(String[] args) {
    Krus krus = new Krus();
    // 取得邊陣列
    EdgeData[] edges = krus.getEdges();
    // 排序邊
    krus.sort(edges);
    System.out.println(Arrays.toString(edges));
    // 存放每個頂點相連的最短路徑
    List<EdgeData> list = new ArrayList<>();
    for (int i = 0; i < edges.length; i++) {
      int parent1 = krus.find(edges[i].getStart());
      int parent2 = krus.find(edges[i].getEnd());
      // 若2個頂點的爸爸不同，才加入list中
      if (parent1 != parent2) {
        krus.union(edges[i].getStart(), edges[i].getEnd());
        list.add(edges[i]);
      }
    }
    System.out.println("list = "  + list);
  }
}
class EdgeData {
  private int start;
  private int end;
  private int weight;

  public int getStart() {
    return start;
  }

  public void setStart(int start) {
    this.start = start;
  }

  public int getEnd() {
    return end;
  }

  public void setEnd(int end) {
    this.end = end;
  }

  public int getWeight() {
    return weight;
  }

  public void setWeight(int weight) {
    this.weight = weight;
  }

  @Override
  public String toString() {
    return "{" +
        "s=" + start +
        ", e=" + end +
        ", w=" + weight +
        '}';
  }
}
{% endhighlight %}

[1]: {% link _pages/dataStruct/uni_find.md %}