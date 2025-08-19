---
title: BFS
date: 2025-08-18
keywords: java, BFS
---
## 預先準備
1. matrix二維陣列，1代表連起來(Connected)，0代表不能連。
2. `visted[]`陣列，判斷是否已訪問過。
3. Queue佇列

matrix二維陣列:

|頂點|j=0|j=1|j=2|j=3|
|i=0  |0|1|1|0|
|i=1  |1|0|0|1|
|i=2  |1|0|0|0|
|i=3  |0|1|0|0|

visted陣列

|頂點   |0|1|2|3|
|visted|F|F|F|F|

先把頂點0放入佇列，下圖中，箭頭入是放入佇列的方向，箭頭出是從佇列取出的方向。<br>
![img]({{site.imgurl}}/java_datastruct/bfs1.png)<br>

把頂點設為已訪問，避免重覆訪問。<br>

|頂點   |0|1|2|3|
|visted|~~F~~ <span class="markline">T</span>|F|F|F|

把頂點0從佇列取出來，印出頂點0。<br>
![img]({{site.imgurl}}/java_datastruct/bfs2.png)<br>

查一下matrix表格中，第0列(i=0)代表頂點0，j是相連頂點，1代表相連，j=1與j=2的時候，是1，是相連的，把頂點1與頂點2放入佇列。<br>

|頂點|j=0|j=1|j=2|j=3|
|i=0|0|<span class="markline">1</span>|<span class="markline">1</span>|0|

![img]({{site.imgurl}}/java_datastruct/bfs3.png)<br>

並把頂點1與頂點2設為已訪問。<br>

|頂點   |0|1|2|3|
|visted|T|~~F~~ <span class="markline">T</span>|~~F~~ <span class="markline">T</span>|F|

把頂點1從佇列取出，印出頂點1。<br>
![img]({{site.imgurl}}/java_datastruct/bfs4.png)<br>

查一下matrix表格中，第1列(i=1)代表頂點1，j是相連頂點，1代表相連，j=0與j=3的時候，是1，是相連的，但頂點0已被訪問過，只放入未訪問頂點3放入佇列。<br>

|頂點|j=0|j=1|j=2|j=3|
|i=0  |0|1|1|0|
|i=1  |1|0|0|<span class="markline">1</span>|

![img]({{site.imgurl}}/java_datastruct/bfs5.png)<br>

並把頂點3設為已訪問。<br>

|頂點   |0|1|2|3|
|visted|T|T|T|~~F~~ <span class="markline">T</span>|

把頂點2從佇列取出，印出頂點2。<br>
![img]({{site.imgurl}}/java_datastruct/bfs6.png)<br>

把頂點3從佇列取出，印出頂點3。<br>
![img]({{site.imgurl}}/java_datastruct/bfs7.png)<br>

完整程式碼如下:
{% highlight java linenos %}
import java.util.LinkedList;

public class BFS {
  private int[][] matrix;
  private int vertexLen = 4;
  private boolean[] visted;

  public BFS() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{0, 1, 1, 0};
    matrix[1] = new int[]{1, 0, 0, 1};
    matrix[2] = new int[]{1, 0, 0, 0};
    matrix[3] = new int[]{0, 1, 0, 0};
    visted = new boolean[vertexLen];
  }

  public void bfs() {
    LinkedList<Integer> queue = new LinkedList<>();
    // 因為可能會有頂點互相不連通，所以要使用for，確保每個頂點都訪問到。
    for (int i = 0; i < vertexLen; i++) {
      // 是否訪問過？
      if (!visted[i]) {
      	// 沒訪問過才加入queue
        queue.add(i);
        // 設為已訪問
        visted[i] = true;
      }
      // 若queue不為空就一直迴圈
      while (!queue.isEmpty()) {
      	// 從queue取出頂點
        int vertex = queue.poll();
        // 印出頂點
        System.out.print(vertex + "->");
        // 檢查j欄(column)的頂點是否有相連
        for (int j = 0; j < vertexLen; j++) {
          // 沒有被訪問 並且相連
          if (!visted[j] && matrix[vertex][j] == 1) {
          	// 加入queue
            queue.add(j);
            // 設為已訪問
            visted[j] = true;
          }
        }
      }
    }
  }

  public static void main(String[] args) {
    BFS bfs = new BFS();
    bfs.bfs();
  }
}
{% endhighlight %}
```
0->1->2->3->
```