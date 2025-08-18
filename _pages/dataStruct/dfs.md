---
title: DFS
date: 2025-08-18
keywords: java, DFS
---

{% highlight java linenos %}
public class DFS {
  private int[][] matrix;
  private int vertexLen = 4;
  private boolean[] visted;

  public DFS() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{0, 1, 1, 0};
    matrix[1] = new int[]{1, 0, 0, 1};
    matrix[2] = new int[]{1, 0, 0, 0};
    matrix[3] = new int[]{0, 1, 0, 0};
    visted = new boolean[vertexLen];
  }
  public void dfs() {
    for (int i = 0; i < vertexLen; i++) {
      if (!visted[i]) {
       dfs(i);
      }
    }
  }
  public void dfs(int vertex) {
    visted[vertex] = true;
    System.out.print(vertex + "->");
    for (int j = 0; j < vertexLen; j++) {
      if (!visted[j] && matrix[vertex][j] == 1) {
        dfs(j);
      }
    }
  }

  public static void main(String[] args) {
    DFS dfs = new DFS();
    dfs.dfs();
  }
}

{% endhighlight %}