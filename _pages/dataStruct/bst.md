---
title: BST
date: 2025-07-31
keywords: java,Binary Search Tree
---
左子節點小於中間節點，右子節點大於中間節點。

## 新增
中序的方式來遍歷樹，是由小到大，因為左子節點小於中間節點，右子節點大於中間節點。

新增的時候要判斷，若樹為空，直接把節點作為root根節點。<br>
若樹不為空，判斷新增節點是小於中間節點還是大於中間節點。<br>
小於中間節點，判斷左節點是否為null，若為null，就把新節點接上左節點。<br>
若左節點不是null，繼續往左子樹找適合的位置來新增。<br>
大於中間節點，判斷右節點是否為null，若為null，就把新節點接上右節點。<br>
若右節點不是null，繼續往右子樹找適合的位置來新增。<br>

建立二個類別，分別為class Node ，與class BinarySearchTree 。<br>
Node處理非root的新增。<br>
BinarySearchTree處理root節點的新增。<br>

{% highlight java linenos %}
public class BSTree {
  public static void main(String[] args) {
    BinarySearchTree binarySearchTree = new BinarySearchTree();
    binarySearchTree.add(new Node(10));
    binarySearchTree.add(new Node(5));
    binarySearchTree.add(new Node(12));
    binarySearchTree.add(new Node(20));
    binarySearchTree.add(new Node(18));
    binarySearchTree.inOrder();
  }
}
class BinarySearchTree {
  private Node root;
  public void add(Node node) {
    if (root == null) {
      // 如果是空樹，就把根節點設為node
      root = node;
    } else {
      // 如果不是空樹，就去找子節點，可新增的位罝
      root.add(node);
    }
  }
  public void inOrder() {
    if (root != null) {
      root.inOrder();
    }
  }
}
class Node {
  int id;
  Node left;
  Node right;
  public Node(int id) {
    this.id = id;
  }
  @Override
  public String toString() {
    return "id=" + id ;
  }
  public void inOrder() {
    if (this.left != null) {
      this.left.inOrder();
    }
    System.out.println(this);
    if (this.right != null) {
      this.right.inOrder();
    }
  }
  public void add(Node node) {
    if (node == null) return;
    // 小於新增在左子樹
    if (node.id < this.id) {
      // 左子樹是葉子節點，才新增
      if (this.left == null) {
        this.left = node;
      } else { // 不是葉子，就繼續往左子樹找
        this.left.add(node);
      }
    } else {
      // 大於、等於，新增在右子樹
      // 右子樹是葉子節點，才新增
      if (this.right == null) {
        this.right = node;
      } else { // 不是葉子，就繼續往右子樹找
        this.right.add(node);
      }
    }
  }
}
{% endhighlight %}

## 刪除
### root節點刪除
1. 先判斷要刪除的節點是不是root節點。
2. 不是root，那就是子節點。

### 子節點刪除
單向鏈結串列刪除時，要先找到刪除節點的前一個節點，因為單向鏈結串列無法往回走。<br>
樹的刪除也是一樣，要先找到「要刪除節點」的「父節點」。<br>

#### 葉子節點刪除
若是要刪除的節點是葉子，檢查要刪除的節點是左子節點，還是右子節點。<br>
直接設為null。<br>
左子節點<br>
{% highlight java linenos %}
parent.left = null;
{% endhighlight %}
右子節點<br>
{% highlight java linenos %}
parent.right = null;
{% endhighlight %}
