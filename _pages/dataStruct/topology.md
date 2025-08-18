---
title: 拓撲排序
date: 2025-08-17
keywords: java, Topology Sort
---
所謂的拓撲排序中的「排序」，並非是由小到大或由大到小。而是誰先誰後，是先後的問題。<br>

## 無向圖
### 分支度


## 有向圖
### 出分支度
### 入分支度

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