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