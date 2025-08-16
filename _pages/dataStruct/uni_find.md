---
title: Union Find
date: 2025-08-16
keywords: java, Union Find
---
Union Find是一個找爸爸的演算法。<br>
常常用來處理圖中是否有環。<br>

## 簡易Union Find
先介紹簡易。<br>

下面有三個頂點:<br>
0, 1, 2<br>

0 → 1 0與1相連，請想像成0的爸爸是1，也就是箭頭指向的是爸爸。<br>

![img]({{site.imgurl}}/java_datastruct/uni_s1.png)<br>

以下是記錄爸爸表格，預設都為-1，-1代表沒有爸爸，各自是獨立。<br>

|頂點   |0|1|2|
|parent|-1|-1|-1|

0 → 1 0與1相連，0的爸爸是1，表格修改如下:<br>

|頂點   |0|1|2|
|parent|<span class="markline">1</span>|-1|-1|

1 → 2 1的爸爸是2，修改parent表格。

![img]({{site.imgurl}}/java_datastruct/uni_s2.png)<br>

|頂點   |0|1|2|
|parent|1|<span class="markline">2</span>|-1|

透過parent表格，可以從parent[0]找到爸爸是1，再從parent[1]找到爸爸是2，可以知道頂點0與1與2是同一個家族的，因為0的爸爸是1，1的爸爸是2。

以下參數為頂點vertex，若頂點沒有爸爸，是-1，就會傳回頂點本身。<br>
若有爸爸，就會一路尋找爸爸的爸爸，直到已經是最頂的祖先(沒有爸爸，是-1)，傳回最頂的祖先。
{% highlight java linenos %}
  public int find(int vertex) {
    while (parent[vertex] != -1) {
      // 把爸爸傳給vertex，繼續找爸爸，直到-1找不到爸爸就離開迴圈。
      vertex = parent[vertex];
    }
    return vertex;
  }
{% endhighlight %}

2 → 0 若要把2的爸爸設為0，就會形成一個圈圈，也就是一個環，如下圖:<br>

![img]({{site.imgurl}}/java_datastruct/uni_s3.png)<br>

怎麼判斷是一個環呢？也就是頂點0的最頂的祖先是頂點2。頂點2沒有爸爸，find(2)方法會傳回頂點自己，也就是2。<br>
```
find(0) = 2
find(2) = 2
```
若2個頂點找到的值都相同，就是形成一個環，就不加入parent表格就不變更了。<br>
以下程式碼，參數為2個頂點，分別是頂點1(vertex1)與頂點2(vertex2)。<br>
{% highlight java linenos %}
  // vertex1 → vertex2，要把箭頭指向的vertex2設為爸爸
  public void union(int vertex1, int vertex2) {
    // vertex1的最頂祖先
    int parent1 = find(vertex1);
    // vertex2的最頂祖先
    int parent2 = find(vertex2);
    // 若不相同，不會形成環
    if (parent1 != parent2) {
      // 把vertex2設為爸爸
      parent[vertex1] = vertex2;
    }
  }
{% endhighlight %}

完整程式碼
{% highlight java linenos %}
public class SimpleUniFind {
  private int[] parent;
  private int vertexNum = 4;

  public static void main(String[] args) {
    SimpleUniFind uniFind = new SimpleUniFind();
    uniFind.union(0,1);
    uniFind.union(1,2);
    uniFind.union(2,0);
    uniFind.print();
  }
  public SimpleUniFind() {
    parent = new int[vertexNum];
    Arrays.fill(parent,-1);
  }
  public int find(int vertex) {
    while (parent[vertex] != -1) {
      vertex = parent[vertex];
    }
    return vertex;
  }
  public void union(int vertex1, int vertex2) {
    int parent1 = find(vertex1);
    int parent2 = find(vertex2);
    if (parent1 != parent2) {
      parent[vertex1] = vertex2;
    }
  }
  public void print() {
    System.out.println(Arrays.toString(parent));
  }
}
{% endhighlight %}

## Union find
parent表格預設爸爸就是頂點自己。

|頂點   |0|1|2|
|parent|0|1|2|

下面有三個頂點:<br>
0, 1, 2<br>

0 → 1 0與1相連，請想像成0的爸爸是1，也就是箭頭指向的是爸爸。<br>

![img]({{site.imgurl}}/java_datastruct/uni_s1.png)<br>

修改0的爸爸是1，預設爸爸都是頂點本身自己，<span class="markline">若不是自己，就代表有爸爸。</span><br>

|頂點   |0|1|2|
|parent|<span class="markline">1</span>|1|2|

1 → 2 1的爸爸是2，修改parent表格。

![img]({{site.imgurl}}/java_datastruct/uni_s2.png)<br>

|頂點   |0|1|2|
|parent|1|<span class="markline">2</span>|2|

透過parent表格，可以從parent[0]找到爸爸是1，再從parent[1]找到爸爸是2，可以知道頂點0與1與2是同一個家族的，因為0的爸爸是1，1的爸爸是2。

以下參數為頂點vertex，若頂點的爸爸是頂點本身，就不進入while迴圈，傳回頂點本身。<br>
若有爸爸，就會一路尋找爸爸的爸爸，直到已經是最頂的祖先(爸爸是頂點本身，離開迴圈)，傳回最頂的祖先。
{% highlight java linenos %}
  public int find(int vertex) {
    // 離開迴圈的條件:爸爸是頂點本身，離開迴圈
    // 進入迴圈的條件:爸爸不是頂點本身
    while (parent[vertex] != vertex) {
      // 把爸爸傳給vertex，繼續找爸爸
      vertex = parent[vertex];
    }
    return vertex;
  }
{% endhighlight %}

另一種寫法是遞迴寫法，二者意思相同。
{% highlight java linenos %}
  public int find(int vertex) {
    if (parent[vertex] == vertex) {
      return vertex;
    }
    return find(parent[vertex]);
  }
{% endhighlight %}

完整程式碼
{% highlight java linenos %}
public class UniFind {
  private int[] parent;
  private int vertexNum = 4;

  public static void main(String[] args) {
    UniFind uniFind = new UniFind();
    uniFind.union(0,1);
    uniFind.union(1,2);
    uniFind.union(2,0);
  }
  public UniFind() {
    parent = new int[vertexNum];
    for (int i = 0; i < vertexNum; i++) {
      parent[i] = i;
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
      parent[vertex1] = vertex2;
    }
  }
}
{% endhighlight %}

## 關於爸爸
之前頂點1 → 頂點2，都把頂點2設為爸爸，但也可以相反。<br>
頂點1 → 頂點2，把頂點1設為爸爸，結果都會是相同的，不管是頂點1設為爸爸，或頂點2設為爸爸。<br>
{% highlight java linenos %}
  public void union(int vertex1, int vertex2) {
    int parent1 = find(vertex1);
    int parent2 = find(vertex2);
    if (parent1 != parent2) {
      parent[vertex2] = vertex1;
    }
  }
{% endhighlight %}