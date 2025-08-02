---
title: AVL高度
date: 2025-08-02
keywords: java, AVL Tree
---
## 左右子樹的高度不能超過1
AVL樹左子樹與右子樹相減高度不能超過1，要小於等於1。<br>
![img]({{site.imgurl}}/java_datastruct/avl_h.png)<br>

## 取得高度
左子樹為null，傳回0，否則傳回左子樹高度。<br>
右子樹為null，傳回0，否則傳回右子樹高度。<br>
\+1，是加上根節點本身的高度。<br>

![img]({{site.imgurl}}/java_datastruct/avl_h2.png)

{% highlight java linenos %}
  public int height() {
    return Math.max(left == null ? 0 : left.height(),
        right == null ? 0 : right.height()) + 1;
  }
{% endhighlight %}

## 左右子樹高度
此處沒加1，是不含根節點本身的高度。

{% highlight java linenos %}
  public int leftHeight() {
    if (left == null) {
      return 0;
    } else {
     return left.height();
    }
  }

  public int rightHeight() {
    if (right == null) {
      return 0;
    } else {
      return right.height();
    }
  }
{% endhighlight %}

## 完整程式碼
{% highlight java linenos %}
public class avlTree {
  public static void main(String[] args) {
    BinarySearchTree bst = new BinarySearchTree();
    int[] arr = {10, 5, 11, 12, 13, 14};
    for (int i = 0; i < arr.length; i++) {
      bst.add(new Node(arr[i]));
    }
    int h = bst.root.height();
    System.out.println(h);
    int left_h = bst.root.leftHeight();
    System.out.println(left_h);
    int right_h = bst.root.rightHeight();
    System.out.println(right_h);
  }
}
class BinarySearchTree {
  public Node root;
  public void add(Node node) {
    if (root == null) {
      // 如果是空樹，就把root設為node
      root = node;
    } else {
      // 如果不是空樹，就去找子樹，可新增的位罝
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
  Node left;
  Node right;
  int value;

  public Node(int value) {
    this.value = value;
  }
  
  public int height() {
    return Math.max(left == null ? 0 : left.height(),
        right == null ? 0 : right.height()) + 1;
  }

  public int leftHeight() {
    if (left == null) {
      return 0;
    } else {
     return left.height();
    }
  }

  public int rightHeight() {
    if (right == null) {
      return 0;
    } else {
      return right.height();
    }
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
    // 小於目前節點，新增在左子樹
    if (node.value < this.value) {
      // 左子樹是葉子節點，才新增
      if (this.left == null) {
        this.left = node;
      } else { // 不是葉子，就繼續往左子樹找
        this.left.add(node);
      }
    } else {
      // 大於、等於目前節點，新增在右子樹
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