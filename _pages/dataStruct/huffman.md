---
title: 霍夫曼樹
date: 2025-08-20
keywords: java, Huffman
---
{% highlight java linenos %}
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Huffman {
  public static void main(String[] args) {
    createHuffman();
  }
  public static void createHuffman() {
    int[] data = {13, 7, 8, 3, 29, 6, 1};
    List<Node> list = new ArrayList<>();
    for (int i = 0; i < data.length; i++) {
      Node node = new Node(data[i]);
      list.add(node);
    }

    while (list.size() > 1) {
      Collections.sort(list);
      Node leftNode = list.get(0);
      Node rightNode = list.get(1);
      Node parent = new Node(leftNode.value + rightNode.value);
      parent.left = leftNode;
      parent.right = rightNode;
      list.remove(leftNode);
      list.remove(rightNode);
      list.add(parent);
    }
    list.get(0).preOrder();

  }
}
class Node implements Comparable<Node>{
  public int value;
  public Node left;
  public Node right;

  public Node(int value) {
    this.value = value;
  }

  @Override
  public int compareTo(Node o) {
    return this.value - o.value;
  }

  public void preOrder() {
    System.out.println(this.value);
    if (this.left != null) {
      this.left.preOrder();
    }
    if (this.right != null) {
      this.right.preOrder();
    }
  }

  @Override
  public String toString() {
    return "Node{" +
        "value=" + value +
        '}';
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Huffman {
  public static void main(String[] args) {
    String content = "I like like like java do you like a java";
    byte[] bytes = content.getBytes();
    System.out.println(Arrays.toString(bytes));
    List<Node> countList = count(bytes);
    Node huffman = createHuffman(countList);
    //huffman.preOrder();
    StringBuilder sb = new StringBuilder();
    getCode(huffman, "", sb);
    System.out.println(huffMap);
    byte[] rtn = zip(content.getBytes(), huffMap);
    System.out.println(Arrays.toString(rtn));
  }

  static Map<Byte, String> huffMap = new HashMap<>();
  private static String byteToBitString(boolean flag, byte b) {
    int temp = b;
    if (flag) {
      temp |= 256;
    }
    String str = Integer.toBinaryString(temp);
    if (flag) {
      return str.substring(str.length() - 8);
    } else {
      return str;
    }
  }
  private static byte[] zip(byte[] bytes, Map<Byte, String> huffMap) {
    StringBuilder sb = new StringBuilder();
    for (Byte b : bytes) {
      sb.append(huffMap.get(b));
    }
    int len = 0;
    if (sb.length() % 8 == 0) {
      len = sb.length() / 8;
    } else {
      len = sb.length() / 8 + 1;
    }
    byte[] huffBytes = new byte[len];
    int index = 0;
    for (int i = 0; i < sb.length(); i += 8) {
      String strByte = null;
      if (i + 8 > sb.length()) {
        strByte = sb.substring(i);
      } else {
        strByte = sb.substring(i, i + 8);
      }
      huffBytes[index++] = (byte) Integer.parseInt(strByte, 2);
    }
    return huffBytes;
  }

  private static void getCode(Node node, String code, StringBuilder sb) {
    StringBuilder sb2 = new StringBuilder(sb);
    sb2.append(code);
    if (node != null) {
      if (node.data == null) {
        getCode(node.left, "0", sb2);
        getCode(node.right, "1", sb2);
      } else {
        huffMap.put(node.data, sb2.toString());
      }
    }
  }

  public static Node createHuffman(List<Node> list) {
    while (list.size() > 1) {
      Collections.sort(list);
      Node leftNode = list.get(0);
      Node rightNode = list.get(1);
      Node node = new Node(null, leftNode.weight + rightNode.weight);
      node.left = leftNode;
      node.right = rightNode;
      list.remove(leftNode);
      list.remove(rightNode);
      list.add(node);
    }
    return list.get(0);
  }

  public static List<Node> count(byte[] bytes) {
    List<Node> list = new ArrayList<>();
    Map<Byte, Integer> map = new HashMap<>();
    for (int i = 0; i < bytes.length; i++) {
      Byte key = bytes[i];
      if (!map.containsKey(key)) {
        map.put(key, 1);
      } else {
        int count = map.get(key);
        map.put(key, count + 1);
      }
    }

    Iterator it = map.keySet().iterator();
    while (it.hasNext()) {
      Byte key = (Byte) it.next();
      Integer value = map.get(key);
      list.add(new Node(key, value));
    }
    return list;
  }
}

class Node implements Comparable<Node> {
  public Byte data;
  public int weight;
  public Node left;
  public Node right;

  public Node(Byte data, int weight) {
    this.data = data;
    this.weight = weight;
  }

  @Override
  public int compareTo(Node o) {
    return this.weight - o.weight;
  }

  public void preOrder() {
    System.out.println(this);
    if (this.left != null) {
      this.left.preOrder();
    }
    if (this.right != null) {
      this.right.preOrder();
    }
  }

  @Override
  public String toString() {
    return "Node{" +
        "data=" + data +
        ", weight=" + weight +
        '}';
  }
}
{% endhighlight %}