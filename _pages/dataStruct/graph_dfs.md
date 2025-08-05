---
title: 圖DFS
date: 2025-08-04
keywords: java, Graph
---

{% highlight java linenos %}
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class Graph {
  private int[][] graph;
  private boolean[] visited;
  private int numOfEdges = 0;
  // 參數為幾個頂點
  public Graph(int vertex) {
    graph = new int[vertex][vertex];
    visited = new boolean[vertex];
  }
  public void print() {
    for (int i = 0; i < graph.length; i++) {
      for (int j = 0; j < graph[i].length; j++) {
        System.out.print(graph[i][j] + "\t");
      }
      System.out.println();
    }
  }
  public static void main(String[] args) {
    Graph graph1 = new Graph(8);
    graph1.insertEdges(0, 1);
    graph1.insertEdges(0, 2);
    graph1.insertEdges(1, 3);
    graph1.insertEdges(1, 4);
    graph1.insertEdges(3, 7);
    graph1.insertEdges(4, 7);
    graph1.insertEdges(2, 5);
    graph1.insertEdges(2, 6);
    graph1.insertEdges(5, 6);
    graph1.print();
    //graph1.dfs();
    //graph1.bfs();
  }
  public void insertEdges(int v1, int v2) {
    graph[v1][v2] = 1;
    graph[v2][v1] = 1;
    numOfEdges++;
  }
  public void bfs() {
    LinkedList<Integer> queue = new LinkedList<>();
    for (int i = 0; i < graph.length; i++) {
      if (!visited[i]) {
        queue.add(i);
        visited[i] = true;
      }
      while (!queue.isEmpty()) {
        int vertex = queue.poll();
        System.out.print(vertex + " -> ");
        for (int j = 0; j < graph[vertex].length; j++) {
          if (!visited[j] && graph[vertex][j] != 0) {
            queue.add(j);
            visited[j] = true;
          }
        }
      }
    }
  }
  public void dfs() {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    dfs(visited, 0, path, result);
//    System.out.println(result);
  }
  public void dfs(boolean[] visited, int vertex, List<Integer> path, List<List<Integer>> result) {
    // false代表沒被訪問，反向!，把沒被訪問設為TRUE
    if (!visited[vertex]) {
      System.out.print(vertex + " -> ");
//      path.add(vertex);
      visited[vertex] = true;
//      if (vertex == graph.length - 1) {
//        result.add(new ArrayList<>(path));
//        return;
//      }
      for (int i = 0; i < graph[vertex].length; i++) {
        if (graph[vertex][i] != 0 && !visited[i]) {
          dfs(visited, i, path, result);
          //visited[i] = false;
          //path.remove(path.size() - 1);
        }
      }
    }
  }

}
{% endhighlight %}