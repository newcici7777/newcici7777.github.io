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
    String content = "I like like like abc";
    byte[] bytes = content.getBytes();
    System.out.println(Arrays.toString(bytes));
    List<Node> countList = count(bytes);
    System.out.println(countList);
    Node huffman = createHuffman(countList);
    huffman.preOrder();
    StringBuilder sb = new StringBuilder();
    getCode(huffman, "", sb);
    System.out.println(huffMap);
    byte[] zipbytes = zip(content.getBytes(), huffMap);
    System.out.println("zipbytes:" + Arrays.toString(zipbytes));
    byte[] source = decode(huffMap, zipbytes);
    System.out.println(new String(source));
  }

  static Map<Byte, String> huffMap = new HashMap<>();

  private static String byteToBitString(byte b) {
    int temp = b;
    temp |= 256;
    String str = Integer.toBinaryString(temp);
    // 只取倒數8位
    return str.substring(str.length() - 8);
  }

  private static byte[] decode(Map<Byte, String> huffMap, byte[] bytes) {
    StringBuilder sb = new StringBuilder();
    // bytes.length - 1 記錄最後一個元素的字串長度
    // 因為字串轉二進位，00001，前面的0都會被去掉，要記錄字串的長度，推算出前面有幾個0
    // bytes.length - 2 是最後一個元素
    int endIndex = bytes.length - 2;
    // 注意，i小於endIndex，不處理endIndex的元素
    for (int i = 0; i < endIndex; i++) {
      byte b = bytes[i];
      sb.append(byteToBitString(b));
    }
    // 處理最後一個元素endIndex
    byte lastByte = bytes[endIndex];
    // 若為負數
    if (lastByte < 0) {
      // 用原本的方式轉出
      sb.append(byteToBitString(lastByte));
    } else { // 若為正整數
      // 先轉成二進位字串，例:0110，就會轉成110
      String str = Integer.toBinaryString(lastByte);
      // 取得endIndex元素的字串長度，例:0110，大小為4
      byte lastCount = bytes[bytes.length - 1];
      // 要使用StringBuffer.insert()插入工具
      StringBuffer sb2 = new StringBuffer(str);
      // lastCount = 0110原本大小為4 - 110二進位轉出來大小為3 = 1，前面還要補一個0
      lastCount -= str.length();
      while (lastCount > 0) {
        // 在索引0插入零，之後的字串往後移
        sb2.insert(0, "0");
        lastCount--;
      }
      // 補0的字串，加在sb
      sb.append(sb2);
    }

    Map<String, Byte> map = new HashMap<>();
    for (Map.Entry<Byte, String> entry : huffMap.entrySet()) {
      map.put(entry.getValue(), entry.getKey());
    }

    List<Byte> list = new ArrayList<>();
    for (int i = 0; i < sb.length(); ) {
      int count = 1;
      boolean flag = true;
      Byte b = null;
      while (flag) {
        String key = sb.substring(i, i + count);
        b = map.get(key);
        if (b == null) {
          count++;
        } else {
          flag = false;
        }
      }
      list.add(b);
      i += count;
    }
    byte b[] = new byte[list.size()];
    for (int i = 0; i < b.length; i++) {
      b[i] = list.get(i);
    }
    return b;
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
    // +1是因為要記錄最後一個元素的大小
    byte[] huffBytes = new byte[len + 1];
    // 若為-1代表，最後一個元素是負數 或剛好大小就為8
    huffBytes[huffBytes.length - 1] = -1;
    int index = 0;
    for (int i = 0; i < sb.length(); i += 8) {
      String strByte = null;
      if (i + 8 > sb.length()) {
        strByte = sb.substring(i);
        // 儲存最後一個元素的字串大小
        huffBytes[huffBytes.length - 1] = (byte) strByte.length();
        System.out.println("end = " + strByte);
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