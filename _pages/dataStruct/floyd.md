---
title: Floyd
date: 2025-08-15
keywords: java, Floyd
---
比較i → j的距離，與i → k → j的距離，誰比較短？

k是一個中間頂點，而i → j是一個直達，不經過k這個中間頂點的路徑。

但如果i → k → j更短呢？

下圖中，0 → 2的距離為10，0 → 1 → 2的距離為3，很明顯0 → 1 → 2的距離更短。

![img]({{site.imgurl}}/java_datastruct/floyd1.png)

preVertex表格是記錄路徑的。<br>
row列是i，column欄是j。<br>

|   |j=0|j=1|j=2|
|i=0|0|0|0|
|i=1|1|1|1|
|i=2|2|2|2|

i為頂點0，j為頂點1，從頂點0 → 頂點1，i為出發頂點，j為終點。<br>

![img]({{site.imgurl}}/java_datastruct/floyd_path1.png)

頂點1的前一個頂點為頂點0。<br>

![img]({{site.imgurl}}/java_datastruct/floyd_path2.png)<br>

i為0，j為2，就是從頂點0 → 頂點2，i為出發頂點，j為終點。<br>

![img]({{site.imgurl}}/java_datastruct/floyd_path3.png)<br>

頂點2的前一個頂點為頂點0。<br>

![img]({{site.imgurl}}/java_datastruct/floyd_path4.png)<br>

從上面的解釋可以知道，預設路徑表中，終點為j，它的前一個頂點預設為出發點i，預設i為前一個頂點。

|出發\\終點|j=0|j=1|j=2|
|i=0|0|0|0|
|i=1|1|1|1|
|i=2|2|2|2|

## matrix表格
matrix表格是記錄各個頂點間距離，如下:<br>

|   |j=0|j=1|j=2|
|i=0|0|1|10|
|i=1|1|0|2|
|i=2|10|2|0|

![img]({{site.imgurl}}/java_datastruct/floyd1.png)

其中，對角線為0，代表0 → 0，頂點0到頂點0，是0距離。<br>
1 → 1，頂點1到頂點1，是0距離。<br>
2 → 2，頂點2到頂點2，是0距離。<br>

## 中間頂點k為0
當出發頂點i是0，當中間頂點k是0，對映j=0,1,2。<br>

出發頂點i → 中間頂點k → j終點 <br>

0→0→0，與0→0→1，與0→0→2，我都視為0→0，0→1，0→2，沒有變化。

當出發頂點i是1，當中間頂點k是0，對映j=0,1,2

### 1→0→0
1→0→0，分解成2種路徑，分別是1→0(i=1,j=0)，與0→0(i=0,j=0)，i是出發頂點，j是終點，查下表。

|出發\\終點|j=0|j=1|j=2|
|i=0|<span class="markline">0</span>|1|10|
|i=1|<span class="markline">1</span>|0|2|
|i=2|10|2|0|

1→0，距離為1。<br>
0→0，距離為0。<br>

仍是沒有比1→0的距離1短，不更新matrix表格與preVertex表格。

### 1→0→1
1→0→1，分解成2種路徑，分別是1→0(i=1,j=0)，與0→1(i=0,j=1)，i是出發頂點，j是終點，查下表。

|出發\\終點|j=0|j=1|j=2|
|i=0|0|<span class="markline">1</span>|10|
|i=1|<span class="markline">1</span>|0|2|
|i=2|10|2|0|

1→0，距離為1。<br>
0→1，距離為1。<br>

1→0→1 = 相加距離為2，仍是沒有比1→1的距離0短，不更新matrix表格與preVertex表格。

### 1→0→2
1→0→2，分解成2種路徑，分別是1→0(i=1,j=0)，與0→2(i=0,j=2)，i是出發頂點，j是終點，查下表。

|出發\\終點|j=0|j=1|j=2|
|i=0|0|1|<span class="markline">10</span>|
|i=1|<span class="markline">1</span>|0|2|
|i=2|10|2|0|

1→0，距離為1。<br>
0→2，距離為10。<br>

1→0→2 = 相加距離為11，仍是沒有比1→2的距離2短，不更新matrix表格與preVertex表格。

## 中間頂點k為1
當出發頂點i是0，當中間頂點k是1，對映j=0,1,2。

出發頂點i → 中間頂點k → j終點 <br>

0→1→0，與0→1→1，與0→1→2，有二個頂點相同的，等同於0→1，等於沒有中間頂點，而是從頂點0直達頂點1，跟沒有透過中間頂點的結果是一樣，距離不會更小，所以我只注重在3個頂點不同。

### 0→1→2
0→1→2，分解成2種路徑，分別是0→1(i=0,j=1)，與1→2(i=1,j=2)，i是出發頂點，j是終點，查下表。

|出發\\終點|j=0|j=1|j=2|
|i=0|0|<span class="markline">1</span>|10|
|i=1|1|0|<span class="markline">2</span>|
|i=2|10|2|0|

0→1，距離為1。<br>
1→2，距離為2。<br>

0→1→2 = 1 + 2 相加距離為3，比0→2的距離10更短，更新matrix表格與preVertex表格。

更新後的表格如下

|matrix|j=0|j=1|j=2|
|i=0|0|1|<span class="markline">3</span>|
|i=1|1|0|2|
|i=2|10|2|0|

preVertex的更新，只在乎「中間頂點」到「終點」的「前一個頂點」，也就是k中間頂點→j終點，先根據下表找出(k=1,j=2)，`[1,2] = 1`，1是前一個頂點。

|出發\\終點|j=0|j=1|j=2|
|i=0|0|0|0|
|i=1|1|1|<span class="markline">1</span>|
|i=2|2|2|2|

接著把出發頂點i→終點j的「前一個頂點」，更新成k中間頂點→j終點的「前一個頂點」，剛才找到的結果為1是前一個頂點。<br>
(i=0,j=2)，更新為1，也就是從0→2，2的前一個頂點是1，而不是原來的0，因為從0→1→2，距離為3，從中間頂點1到終點2，前一個頂點為1會更短。

|出發\\終點|j=0|j=1|j=2|
|i=0|0|0|<span class="markline">1</span>|
|i=1|1|1|1|
|i=2|2|2|2|

要如何從以上路徑表推導路徑？因為路徑表只記錄是否有中間頂點，若出發頂點i→終點j的路徑表為i，代表沒有中間頂點k，i→j已經是最短路徑。

若有中間頂點k，如上表，頂點0→頂點2，前一個頂點是1，而不是頂點0，代表是透過1這個中間頂點，抵達終點2。<br>
拿到中間頂點1，查「出發頂點i」 = 0 → 「中間頂點k」 =1(i=0,k=1)，若(0,1) == i，也就是出發頂點i==0，同時也是前一個頂點，就找到出發頂點，路徑全找到了。<br>

||j=0|j=1|j=2|
|i=0|0|<span class="markline">0</span>|1|
|i=1|1|1|1|
|i=2|2|2|2|

## 程式碼
{% highlight java linenos %}
public class Floyed {
  private int[][] matrix;
  private int[][] preVertex;
  private int vertexLen = 3;
  public Floyed() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{0, 1, 10};
    matrix[1] = new int[]{1, 0, 2};
    matrix[2] = new int[]{10, 2, 0};

    // preVertex尋找前一個頂點，i是出發頂點，j是終點
    preVertex = new int[vertexLen][vertexLen];
    for (int i = 0; i < preVertex.length; i++) {
      // 預設前一個頂點是i出發頂點，若沒有中間頂點
      // i → j，是直達，j頂點的前一個頂點是i
      Arrays.fill(preVertex[i], i);
    }
  }

  public void floyed() {
    int len = 0;
    for (int k = 0; k < vertexLen; k++) {
      for (int i = 0; i < vertexLen; i++) {
        for (int j = 0; j < vertexLen; j++) {
          len = matrix[i][k] + matrix[k][j];
          if (len < matrix[i][j]) {
            matrix[i][j] = len;
            preVertex[i][j] = preVertex[k][j];
          }
        }
      }
    }
  }

  public void print() {
    for (int i = 0; i < matrix.length; i++) {
      for (int j = 0; j < matrix.length; j++) {
        System.out.print(matrix[i][j] + ", ");
      }
      System.out.println();
    }
    System.out.println();
    for (int i = 0; i < matrix.length; i++) {
      for (int j = 0; j < matrix.length; j++) {
        System.out.print(preVertex[i][j] + ", ");
      }
      System.out.println();
    }
  }

  public void printPath(int i, int j) {
    System.out.print(j);
    // 出發頂點i → 終點j，是否有中間頂點
    int k = preVertex[i][j];
    // 若中間頂點k是出發頂點i，就代表前一個頂點，就是出發頂點i。
    // 若中間頂點k「不是」出發頂點i，就代表有中間頂點，繼續找，直到中間頂點為i(出發頂點)。
    while (k != i) {
      System.out.print("->" + k + "->");
      // 繼續找中間頂點，i是出發頂點是固定
      k = preVertex[i][k];
    }
    System.out.print(i);
  }

  public static void main(String[] args) {
    Floyed floy = new Floyed();
    floy.floyed();
    floy.print();
    floy.printPath(2,0);
  }
}
{% endhighlight %}
```
0, 1, 3, 
1, 0, 2, 
3, 2, 0, 

0, 0, 1, 
1, 1, 1, 
1, 2, 2, 
0->1->2
```

## 另一種路徑表程式碼
預設路徑表preVertex全為-1，-1代表沒有中間頂點。

|出發\\終點|j=0|j=1|j=2|
|i=0|-1|-1|-1|
|i=1|-1|-1|-1|
|i=2|-1|-1|-1|




{% highlight java linenos %}
public class Floyed {
  private int[][] matrix;
  private int[][] preVertex;
  private int vertexLen = 3;
  public Floyed() {
    matrix = new int[vertexLen][vertexLen];
    matrix[0] = new int[]{0, 1, 10};
    matrix[1] = new int[]{1, 0, 2};
    matrix[2] = new int[]{10, 2, 0};

    preVertex = new int[vertexLen][vertexLen];
    for (int i = 0; i < preVertex.length; i++) {
      Arrays.fill(preVertex[i], -1);
    }
  }

  public void floyed() {
    int len = 0;
    for (int k = 0; k < vertexLen; k++) {
      for (int i = 0; i < vertexLen; i++) {
        for (int j = 0; j < vertexLen; j++) {
          len = matrix[i][k] + matrix[k][j];
          if (len < matrix[i][j]) {
            matrix[i][j] = len;
            preVertex[i][j] = k;
          }
        }
      }
    }
  }

  public void print() {
    for (int i = 0; i < matrix.length; i++) {
      for (int j = 0; j < matrix.length; j++) {
        System.out.print(matrix[i][j] + ", ");
      }
      System.out.println();
    }
    System.out.println();
    for (int i = 0; i < matrix.length; i++) {
      for (int j = 0; j < matrix.length; j++) {
        System.out.print(preVertex[i][j] + ", ");
      }
      System.out.println();
    }
  }

  public void printPath(int i, int j) {
    if (preVertex[i][j] == -1) {
      System.out.println(i + "->" + j);
      return;
    }
    int k = preVertex[i][j];
    printPath(i,k);
    printPath(k,j);
  }

  public static void main(String[] args) {
    Floyed floy = new Floyed();
    floy.floyed();
    floy.print();
    floy.printPath(0,2);
  }
}
{% endhighlight %}
