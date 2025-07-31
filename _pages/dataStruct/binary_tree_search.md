---
title: 樹的前序中序後序搜尋
date: 2025-07-31
keywords: java,Binary Tree Traversal Search
---
## 想法
### 前序搜尋
先找目前的節點，是否為要搜尋的對象。<br>
若不是，去左子節點找。<br>
左子節點找不到，去右子節點找。<br>

### 中序搜尋
先去左子節點找。<br>
找不到就去目前的節點找。<br>
找不到去右子節點找。<br>
### 後序搜尋
先去左子節點找。<br>
找不到去右子節點找。<br>
找不到就去目前的節點找。<br>

{% highlight java linenos %}
public class TreeTraversal2 {
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
    System.out.println("====== 中序搜尋 =======");
    findNode = binaryTree.inOrderSearch(root, 4);
    System.out.println(findNode);
    System.out.println("====== 後序搜尋 =======");
    findNode = binaryTree.postOrderSearch(root, 3);
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
  public Node inOrderSearch(Node node, int searchId) {
    if (node == null) return null;
    Node rtn = inOrderSearch(node.left, searchId);
    if (rtn != null) {
      return rtn;
    }
    if (node.id == searchId) {
      return node;
    }
    return inOrderSearch(node.right, searchId);
  }
  public Node postOrderSearch(Node node, int searchId) {
    if (node == null) return null;
    Node rtn = null;
    rtn = postOrderSearch(node.left, searchId);
    if (rtn != null) {
      return rtn;
    }
    rtn = postOrderSearch(node.right, searchId);
    if (rtn != null) {
      return rtn;
    }
    if (node.id == searchId) {
      rtn = node;
    }
    return rtn;
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