---
title: 樹的前序中序後序遍歷
date: 2025-07-31
keywords: java,Binary Tree Traversal
---
Prerequisites:

- [遞迴][1]

## 版本1
{% highlight java linenos %}
public class TreeTraversal {
  public static void main(String[] args) {
    BinaryTree binaryTree = new BinaryTree();
    Node root = binaryTree.createTree();
    System.out.println("====== 前序 =======");
    binaryTree.preOrder(root);
    System.out.println("====== 中序 =======");
    binaryTree.inOrder(root);
    System.out.println("====== 後序 =======");
    binaryTree.postOrder(root);
    System.out.println("====== 前序搜尋 =======");
    Node findNode = binaryTree.preOrderSearch(root, 5);
    System.out.println(findNode);
  }
}
class BinaryTree {
  private Node root;
  public BinaryTree() {
  }
  public BinaryTree(Node root) {
    this.root = root;
  }
  public Node createTree() {
    Node node1 = new Node(1, "Bill");
    Node node2 = new Node(2, "Alice");
    Node node3 = new Node(3, "Mary");
    Node node4 = new Node(4, "Tom");
    Node node5 = new Node(5, "Alex");
    node1.left = node2;
    node1.right = node3;
    node3.left = node4;
    node3.right = node5;
    return node1;
  }
  public void preOrder(Node node) {
    if (node == null) return;
    System.out.println(node);
    preOrder(node.left);
    preOrder(node.right);
  }
  public void inOrder(Node node) {
    if (node == null) return;
    inOrder(node.left);
    System.out.println(node);
    inOrder(node.right);
  }
  public void postOrder(Node node) {
    if (node == null) return;
    postOrder(node.left);
    postOrder(node.right);
    System.out.println(node);
  }
  public Node preOrderSearch(Node node, int searchId) {
    if (node == null) return null;
    if (node.id == searchId) {
      return node;
    }
    Node rtn = preOrderSearch(node.left, searchId);
    if (rtn != null) {
      return rtn;
    }
    return preOrderSearch(node.right, searchId);
  }   
}
class Node {
  Node(int id, String name) {
    this.id = id;
    this.name = name;
  }
  public int id;
  public String name;
  public Node left;
  public Node right;

  @Override
  public String toString() {
    return
        "id=" + id +
        ", name=" + name ;
  }
}
{% endhighlight %}

## 版本2
{% highlight java linenos %}
public class TreeTraversal {
  public static void main(String[] args) {
    Node node1 = new Node(1, "Bill");
    Node node2 = new Node(2, "Alice");
    Node node3 = new Node(3, "Mary");
    Node node4 = new Node(4, "Tom");
    Node node5 = new Node(5, "Alex");
    node1.left = node2;
    node1.right = node3;
    node3.left = node4;
    node3.right = node5;

    BinaryTree binaryTree = new BinaryTree(node1);
    System.out.println("====== 前序 =======");
    binaryTree.preOrder();
    System.out.println("====== 中序 =======");
    binaryTree.inOrder();
    System.out.println("====== 後序 =======");
    binaryTree.postOrder();
//    System.out.println("====== 前序搜尋 =======");
//    Node findNode = binaryTree.preOrderSearch(root, 5);
//    System.out.println(findNode);
  }
}
class BinaryTree {
  private Node root;

  public BinaryTree(Node root) {
    this.root = root;
  }

  public void preOrder() {
    this.root.preOrder();
  }
  public void postOrder() {
    this.root.postOrder();
  }
  public void inOrder() {
    this.root.inOrder();
  }

}
class Node {
  Node(int id, String name) {
    this.id = id;
    this.name = name;
  }
  public int id;
  public String name;
  public Node left;
  public Node right;

  @Override
  public String toString() {
    return
        "id=" + id +
        ", name=" + name ;
  }

  public void preOrder() {
    if (this == null) return;
    System.out.println(this);
    if (this.left != null) {
      this.left.preOrder();
    }
    if (this.right != null) {
      this.right.preOrder();
    }
  }
  public void inOrder() {
    if (this == null) return;

    if (this.left != null) {
      this.left.preOrder();
    }

    System.out.println(this);

    if (this.right != null) {
      this.right.preOrder();
    }
  }
  public void postOrder() {
    if (this == null) return;
    if (this.left != null) {
      this.left.preOrder();
    }

    if (this.right != null) {
      this.right.preOrder();
    }
    System.out.println(this);
  }
}
{% endhighlight %}
```
====== 前序 =======
id=1, name=Bill
id=2, name=Alice
id=3, name=Mary
id=4, name=Tom
id=5, name=Alex
====== 中序 =======
id=2, name=Alice
id=1, name=Bill
id=3, name=Mary
id=4, name=Tom
id=5, name=Alex
====== 後序 =======
id=2, name=Alice
id=3, name=Mary
id=4, name=Tom
id=5, name=Alex
id=1, name=Bill
```


[1]: {% link _pages/java/recursive.md %}
