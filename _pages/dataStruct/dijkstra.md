---
title: Dijkstra
date: 2025-08-11
keywords: java, Dijkstra
---
{% highlight java linenos %}
public class Dijkstra {
  private int[][] matrix ;
  private boolean[] visted;
  private int[] preVertex;
  private int[] distance;
  private int vertexLen = 7;
  public static final int MAX = 10000;
  public Dijkstra() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{MAX, 5, 7, MAX, MAX, MAX, 2};
    matrix[1] = new int[]{5, MAX, MAX, 9, MAX, MAX, 3};
    matrix[2] = new int[]{7, MAX, MAX, MAX, 3, MAX, MAX};
    matrix[3] = new int[]{MAX, 9, MAX, MAX, MAX, 4, MAX};
    matrix[4] = new int[]{MAX, MAX, 3, MAX, MAX, 5, 4};
    matrix[5] = new int[]{MAX, MAX, MAX, 4, 5, MAX, 6};
    matrix[6] = new int[]{2, 3, MAX, MAX, 4, 6, MAX};
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
    dijkstra.setStart(6);
    dijkstra.dijkstra(6);
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