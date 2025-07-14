---
title: 單向鏈結串列
date: 2025-07-12
keywords: Java, Single Linked List
---
## 節點
有二個屬性，data與next，next存放下一個節點的記憶體位址，data放存放資料。

## 重要變數說明
head頭節點，不存任何資料，功用是指向串列中第一個節點。<br>

![img]({{site.imgurl}}/java_datastruct/single_linked1.png)

上圖中，head.next指向的節點是第一個節點，也是最後一個節點，最後一個節點指向null。<br>

## 新增節點
在最後一個節點後面，新增節點。
### 變數說明
由於head頭節點的作用是永遠指向串列中第一個節點，不能移動。<br>
所以需要有一個輔助變數curNode，移動到每一個節點，直到找到節點的next為null。<br>
最後一個節點的next指向null就是最後一個節點。<br>

cur是current縮寫，中文是「目前」指向的節點。<br>

### 第一次新增
目前沒有任何節點，只有一個head頭節點，head的next是null。<br>

輔助變數curNode移動到head頭節點。<br>
![img]({{site.imgurl}}/java_datastruct/linked_first_add1.png)

把head的next，指向新的節點。<br>
新節點的next預設為null，詳情請看Node類別。<br>
![img]({{site.imgurl}}/java_datastruct/linked_first_add2.png)

### 不是第一次新增
輔助變數curNode移動到head頭節點。<br>

移動到每一個節點，直到找到最後一個節點的next為null。<br>

把最後一個節點的next指向「新節點」記憶體位址。<br>

![img]({{site.imgurl}}/java_datastruct/linked_add.gif)


### 程式碼
curNode一開始移動到<span class="markline">head</span>，如果第一次新增，是不會有其它節點，所以新增都是指向head。<br>

![img]({{site.imgurl}}/java_datastruct/linked_curNode.png)

{% highlight java linenos %}
class MyLinkedList {
  Node head;
  public MyLinkedList() {
    head = new Node();
  }
  public void add(Node newNode) {
    // 輔助變數curNode移動到head
    Node curNode = head;
    while (true) {
      // 判斷是不是最後一個節點
      if (curNode.next == null) {
        // 找到就離開迴圈，不要再移動
        // 目前的curNode就是最後一個節點
        break;
      }
      // 若不是最後一個節點，curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    // 最後一個節點的next指向新節點
    curNode.next = newNode;
  }
}

class Node {
  public Node() {}
  public Node(int data) {
    this.data = data;
  }
  public int data;
  public Node next;
}
{% endhighlight %}

## 顯示所有節點的data
由於head頭節點的作用是永遠指向串列中第一個節點，不能移動。<br>
所以需要有一個輔助變數curNode，來移動到每一個節點，直到找到節點的next為null。<br>

![img]({{site.imgurl}}/java_datastruct/linked_visit.gif) <br>

輔助變數curNode一開始移動到<span class="markline">第一個節點(head.next)</span>，螢幕顯示目前節點curNode的data，接著移動到下一個節點，直到<span class="markline">移動到null</span>，就離開迴圈。

移動之前，先檢查是否有<span class="markline">第一個節點(head.next)</span>，如果沒有第一個節點就離開方法。
{% highlight java linenos %}
public void visitAll(int n) {
  if (head.next == null) {
    return;
  }
}
{% endhighlight %}

{% highlight java linenos %}
public void visitAll() {
  // 先判斷有沒有第一個節點
  if (head.next == null) {
    return;
  }
  // 輔助變數移動到第一個節點
  Node curNode = head.next;
  while (true) {
    // 如果curNode移動到null，離開迴圈
    if (curNode == null) {
      // 離開迴圈
      break;
    }
    // 螢幕顯示出目前節點的資料data
    System.out.println(curNode.data);
    // 移動到下一個節點
    curNode = curNode.next;
  }
}
{% endhighlight %}

## 完整程式碼
{% highlight java linenos %}
public class Test1 {
  public static void main(String[] args) {
    MyLinkedList list = new MyLinkedList();
    // 新增節點
    Node node1 = new Node(1);
    list.add(node1);
    Node node2 = new Node(2);
    list.add(node2);
    Node node3 = new Node(3);
    list.add(node3);
    // 訪問所有節點，並顯示data
    list.visitAll();
  }
}
class MyLinkedList {
  Node head;
  public MyLinkedList() {
    head = new Node();
  }

  public void add(Node newNode) {
    // 輔助變數先移動到頭節點
    Node curNode = head;
    while (true) {
      // 判斷是不是最後一個節點
      if (curNode.next == null) {
        // 找到就離開迴圈，不要再移動
        // 目前的curNode就是最後一個節點
        break;
      }
      // 若不是最後一個節點，curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    // 最後一個節點的next指向新節點
    curNode.next = newNode;
  }

  public void visitAll() {
    // 先判斷有沒有第一個節點
    if (head.next == null) {
      return;
    }
    // 輔助變數先移動到第一個節點
    Node curNode = head.next;
    while (true) {
      // curNode移動到null
      if (curNode == null) {
        // 離開迴圈
        break;
      }
      // 螢幕顯示出目前節點的資料data
      System.out.println(curNode.data);
      // 移動到下一個節點
      curNode = curNode.next;
    }
  }
}

class Node {
  public Node() {}
  public Node(int data) {
    this.data = data;
  }
  public int data;
  public Node next;
}
{% endhighlight %}

## 由小到大新增
之前的程式碼是新增在最後面，現在我們要依照data的大小，由小到大新增。

先找到串列中比新增節點還「大或是相等」的節點。

因為是單向鏈結串列，每個節點只會知道下一個節點是誰，不會知道前一個節點是誰，所以輔助變數curNode指向在比新增節點還「大或是相等」的節點的<span class="markline">「前一個」</span>，如果curNode指向「大或是相等」的節點，就無法跟前一個節點串在一起。

![img]({{site.imgurl}}/java_datastruct/linked_sort_insert.gif)

{% highlight java linenos %}
public void add(Node newNode) {
  // 輔助變數先移動到頭節點
  Node curNode = head;
  // 判斷是否有找到>=newNode的數字
  boolean flag = false;
  while (true) {
    // 若為最後一個節點
    if (curNode.next == null) {
      // 離開
      break;
    }
    // 判斷下一個節點有沒有>=newNode
    if (curNode.next.data >= newNode.data) {
      flag = true; //找到
      // 找到就離開迴圈，不要再移動
      break;
    }
    // curNode輔助變數移動到下一個節點
    curNode = curNode.next;
  }
  newNode.next = curNode.next;
  curNode.next = newNode;
}
{% endhighlight %}

## 刪除
先找到串列中與參數節點data數值相同的節點。

因為是單向鏈結串列，每個節點只會知道下一個節點是誰，不會知道前一個節點是誰，所以輔助變數curNode指向要刪除節點的<span class="markline">「前一個」</span>，如果curNode指向要刪除的節點，就無法跟前一個節點串在一起。

![img]({{site.imgurl}}/java_datastruct/linked_del1.png)

![img]({{site.imgurl}}/java_datastruct/linked_del2.png)

{% highlight java linenos %}
  public void del(Node delNode) {
    // 輔助變數先移動到頭節點
    Node curNode = head;
    // 判斷是否有找到==delNode的數字
    boolean flag = false;
    while (true) {
      // 若為最後一個節點
      if (curNode.next == null) {
        // 離開
        break;
      }
      // 判斷下一個節點有沒有==delNode
      if (curNode.next.data == delNode.data) {
        flag = true; //找到
        // 找到就離開迴圈，不要再移動
        break;
      }
      // curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    //找到要刪除的節點
    if (flag) {
      curNode.next = curNode.next.next;
    } else {
      System.out.println("找不到要刪除的節點");
    }
  }
{% endhighlight %}

完整程式碼
{% highlight java linenos %}
public class Test1 {
  public static void main(String[] args) {
    MyLinkedList list = new MyLinkedList();
    // 新增節點
    Node node1 = new Node(80);
    list.add(node1);
    Node node2 = new Node(20);
    list.add(node2);
    Node node3 = new Node(10);
    list.add(node3);
    // 訪問所有節點，並顯示data
    list.visitAll();
    System.out.println("=================");
    list.del(node2);  // 刪除20
    list.visitAll();
  }
}
class MyLinkedList {
  Node head;
  public MyLinkedList() {
    head = new Node();
  }

  public void add(Node newNode) {
    // 輔助變數先移動到頭節點
    Node curNode = head;
    // 判斷是否有找到>=newNode的數字
    boolean flag = false;
    while (true) {
      // 若為最後一個節點
      if (curNode.next == null) {
        // 離開
        break;
      }
      // 判斷下一個節點有沒有>=newNode
      if (curNode.next.data >= newNode.data) {
        flag = true; //找到
        // 找到就離開迴圈，不要再移動
        break;
      }
      // curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    newNode.next = curNode.next;
    curNode.next = newNode;
  }

  public void del(Node delNode) {
    // 輔助變數先移動到頭節點
    Node curNode = head;
    // 判斷是否有找到==delNode的數字
    boolean flag = false;
    while (true) {
      // 若為最後一個節點
      if (curNode.next == null) {
        // 離開
        break;
      }
      // 判斷下一個節點有沒有==delNode
      if (curNode.next.data == delNode.data) {
        flag = true; //找到
        // 找到就離開迴圈，不要再移動
        break;
      }
      // curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    //找到要刪除的節點
    if (flag) {
      curNode.next = curNode.next.next;
    } else {
      System.out.println("找不到要刪除的節點");
    }
  }

  public void visitAll() {
    // 先判斷有沒有第一個節點
    if (head.next == null) {
      return;
    }
    // 輔助變數先移動到第一個節點
    Node curNode = head.next;
    while (true) {
      // curNode移動到null
      if (curNode == null) {
        // 離開迴圈
        break;
      }
      // 螢幕顯示出目前節點的資料data
      System.out.println(curNode.data);
      // 移動到下一個節點
      curNode = curNode.next;
    }
  }
}

class Node {
  public Node() {}
  public Node(int data) {
    this.data = data;
  }
  public int data;
  public Node next;
}
{% endhighlight %}

## 修改
節點要進行修正，要有一個id。
{% highlight java linenos %}
class Node {
  public Node() {}
  public Node(int id, int data) {
    this.id = id;
    this.data = data;
  }
  public int id;
  public int data;
  public Node next;
}
{% endhighlight %}

修改程式碼跟刪除的程式碼幾乎一模一樣。

先找到串列中與參數節點id相同的節點。

因為是單向鏈結串列，每個節點只會知道下一個節點是誰，不會知道前一個節點是誰，所以輔助變數curNode指向要修改節點的<span class="markline">「前一個」</span>。

其它程式碼也修改成跟id進行比較，而非data。

程式碼
{% highlight java linenos %}
public class Test1 {
  public static void main(String[] args) {
    MyLinkedList list = new MyLinkedList();
    // 新增節點
    System.out.println("======新增===========");
    Node node1 = new Node(3,80);
    list.add(node1);
    Node node2 = new Node(2,20);
    list.add(node2);
    Node node3 = new Node(1,10);
    list.add(node3);
    // 訪問所有節點，並顯示data
    list.visitAll();
    System.out.println("======刪除===========");
    list.del(node2);  // 刪除20
    list.visitAll();
    System.out.println("======修改===========");
    list.update(new Node(3,10000));
    list.visitAll();
  }
}
class MyLinkedList {
  Node head;
  public MyLinkedList() {
    head = new Node();
  }

  public void add(Node newNode) {
    // 輔助變數先移動到頭節點
    Node curNode = head;
    // 判斷是否有找到>=newNode的數字
    boolean flag = false;
    while (true) {
      // 若為最後一個節點
      if (curNode.next == null) {
        // 離開
        break;
      }
      // 判斷下一個節點id有沒有>=newNode
      if (curNode.next.id >= newNode.id) {
        flag = true; //找到
        // 找到就離開迴圈，不要再移動
        break;
      }
      // curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    newNode.next = curNode.next;
    curNode.next = newNode;
  }

  public void del(Node delNode) {
    // 輔助變數先移動到頭節點
    Node curNode = head;
    // 判斷是否有找到==delNode的id
    boolean flag = false;
    while (true) {
      // 若為最後一個節點
      if (curNode.next == null) {
        // 離開
        break;
      }
      // 判斷下一個節點有沒有==delNode
      if (curNode.next.id == delNode.id) {
        flag = true; //找到
        // 找到就離開迴圈，不要再移動
        break;
      }
      // curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    //找到要刪除的節點
    if (flag) {
      curNode.next = curNode.next.next;
    } else {
      System.out.println("找不到要刪除的節點");
    }
  }

  public void update(Node updateNode) {
    // 輔助變數先移動到頭節點
    Node curNode = head;
    // 判斷是否有找到==updateNode的數字
    boolean flag = false;
    while (true) {
      // 若為最後一個節點
      if (curNode.next == null) {
        // 離開
        break;
      }
      // 判斷下一個節點有沒有==updateNode
      if (curNode.next.id == updateNode.id) {
        flag = true; //找到
        // 找到就離開迴圈，不要再移動
        break;
      }
      // curNode輔助變數移動到下一個節點
      curNode = curNode.next;
    }
    //找到要修改的節點
    if (flag) {
      curNode.next.data = updateNode.data;
    } else {
      System.out.println("找不到要修改的節點");
    }
  }
  public void visitAll() {
    // 先判斷有沒有第一個節點
    if (head.next == null) {
      return;
    }
    // 輔助變數先移動到第一個節點
    Node curNode = head.next;
    while (true) {
      // curNode移動到null
      if (curNode == null) {
        // 離開迴圈
        break;
      }
      String str = String.format("id = %d, data = %d",curNode.id, curNode.data);
      // 螢幕顯示出目前節點的資料data
      System.out.println(str);
      // 移動到下一個節點
      curNode = curNode.next;
    }
  }
}

class Node {
  public Node() {}
  public Node(int id, int data) {
    this.id = id;
    this.data = data;
  }
  public int id;
  public int data;
  public Node next;
}
{% endhighlight %}