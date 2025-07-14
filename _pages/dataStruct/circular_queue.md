---
title: circular queue環狀佇列
date: 2025-07-14
keywords: Java, circular queue環狀佇列
---
## 初始值
陣列的預設值都為0，陣列大小maxsize為6，陣列實際大小len為0，tail與front一開始都是0。

### len
會記錄目前實際大小。

## push與tail
Queue的push，tail一開始指向0，然後新增資料，再把tail \+ 1，len \+ 1。<br>

![img]({{site.imgurl}}/java_datastruct/circular_push.gif) <br>

新增完索引0的值，len會變成1，tail變成1。<br>
新增完索引1的值，len會變成2，tail變成2。<br>
新增完索引2的值，len會變成3，tail變成3。<br>
.<br>
.<br>
.<br>
新增完索引5的值，len會變成6，也就是有新增6個數值，陣列大小為6，不能再新增。<br>

因此要判斷是否為陣列為滿，會用以下的程式碼判斷。
{% highlight java linenos %}
// 是否滿了？
public boolean isFull() {
  // len 等於 6 就回傳true已經滿
  if (len >= maxSize) return true;
  // len < maxSize 
  // len小於6(不包含6)，回傳false沒有滿
  return false;
}
{% endhighlight %}

## pop與front
Queue的pop，front一開始指向0，先把資料取出，然後再把front \+ 1，len \- 1。<br>

![img]({{site.imgurl}}/java_datastruct/circular_pop.gif)

## push與公式
初始值 tail = 0<br>
![img]({{site.imgurl}}/java_datastruct/circular_tail1.png)
<br>
push公式
{% highlight java linenos %}
arr[tail] = val;
tail = (tail + 1) % maxSize;
len++;
{% endhighlight %}

### 第1次新增<br>
將以下的值代入上方push語法中<br>
最大容量為maxLen = 6<br>
tail = 0<br>
arr[tail] = 要新增的值100<br>
tail = (0 + 1) % 6<br>
新增完後，tail = 1 (指向索引1)<br>
len = len + 1，實際大小增加1<br>

![img]({{site.imgurl}}/java_datastruct/circular_tail2.png)

### tail指向陣列最後一個元素
![img]({{site.imgurl}}/java_datastruct/circular_tail3.png)
<br>
將以下的值代入上方push語法中<br>
最大容量為maxLen = 6<br>
tail = 5<br>
arr[5] = 要新增的值 100<br>
tail = (5 + 1) % 6<br>
新增完後，tail = 0 (指向索引0)<br>
len = 2<br>
len = 2 + 1，實際大小變成3。<br>

![img]({{site.imgurl}}/java_datastruct/circular_tail4.png)

## pop與公式
初始值 front = 0<br>

pop語法
{% highlight java linenos %}
int rtn = arr[front];
front = (front + 1) % maxSize;
len--;
{% endhighlight %}

![img]({{site.imgurl}}/java_datastruct/circular_front1.png)

將以下的值代入上方push語法中<br>
最大容量為maxLen = 6<br>
front = 3<br>
先把資料取出arr[front = 3]<br>
然後再把front變數往後移動一格，front = (3 + 1) % 6<br>
資料取出後，front = 4 (指向索引4)<br>
len = 3 - 1，實際大小變成2<br>

![img]({{site.imgurl}}/java_datastruct/circular_front2.png)


假設front指向陣列最後一個元素。

![img]({{site.imgurl}}/java_datastruct/circular_front3.png)

將以下的值代入上方push語法中<br>
最大容量為maxLen = 6<br>
front = 5
先把資料取出arr[front = 5]<br>
然後再把front變數往後移動一格，front = (5 + 1) % 6<br>
資料取出後，front = 0 (指向索引0)<br>
len = 1 - 1，實際大小變成0<br>

![img]({{site.imgurl}}/java_datastruct/circular_front4.png)

## print
print語法跟push與pop語法也是一模一樣，把front代入就行。

![img]({{site.imgurl}}/java_datastruct/circular_print.png)

i為0 .. len實際已占用大小，0<= i < len
{% highlight java linenos %}
(front + i) % maxLen
{% endhighlight %}

以上圖來說

i = 0，front = 3，最大容量為maxLen = 6
```
(3 + 0) % 6 = 3
```

i = 1，front = 3，最大容量為maxLen = 6
```
(3 + 1) % 6 = 4
```

i = 2，front = 3，最大容量為maxLen = 6
```
(3 + 2) % 6 = 5
```

i = 3，front = 3，最大容量為maxLen = 6
```
(3 + 3) % 6 = 0
```

## 程式碼
{% highlight java linenos %}
public class CircularTest {
  public static void main(String[] args) throws Exception {
    CircularQueue queue = new CircularQueue(6);
    System.out.println("=====push=========");
    queue.push(1);
    queue.push(2);
    queue.push(3);
    queue.push(4);
    queue.push(5);
    queue.push(6);
    queue.push(7);
    queue.print();
    System.out.println("======pop========");
    System.out.println(queue.pop());
    System.out.println(queue.pop());
    System.out.println(queue.pop());
    System.out.println(queue.pop());
    System.out.println(queue.pop());
    System.out.println(queue.pop());
    System.out.println(queue.pop());
  }
}
class CircularQueue {
  int[] arr;
  int maxSize;
  int front, tail, len;
  public CircularQueue(int size) {
    arr = new int[size];
    maxSize = size;
    front = 0;
    tail = 0;
    len = 0;
  }
  public boolean isFull() {
    if (len >= maxSize) return true;
    return false; // len < maxSize
  }
  public boolean isEmpty() {
    if (len == 0) return true;
    return false;
  }

  public void push(int val) {
    if(isFull()) {
      System.out.println("Queue已經滿，不能再push");
      return;
    }
    arr[tail] = val;
    tail = (tail + 1) % maxSize;
    len++;
  }
  public int pop() throws Exception {
    if (isEmpty()) {
      throw new Exception("Queue is empty");
    }
    int rtn = arr[front];
    front = (front + 1) % maxSize;
    len--;
    return rtn;
  }
  public int getSize() {
    return len;
  }
  public void print() {
    for (int i = 0; i < getSize(); i++) {
      int index = (front + i) % maxSize;
      System.out.println(arr[index]);
    }
  }
}
{% endhighlight %}