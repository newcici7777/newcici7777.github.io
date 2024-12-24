---
title: circular queue環狀佇列
date: 2024-12-23
keywords: linux, circular queue
---

環狀佇列就是固定大小空間的陣列，循環利用。

## 初始化

front指向第一筆資料

front = 0

tail指向最後一筆資料

tail = 5

![img]({{site.imgurl}}/dataStruct/circular_queue1.jpg)  

## push

### push語法

{% highlight c++ linenos %}
    tail_ = (tail_ + 1) % maxLen;
    // 把值塞入
    data_[tail_] = element;
{% endhighlight %}

### 第1次新增

將以下的值代入上方push語法中

最大容量為maxLen = 6

原本tail = 5 (指向最後一筆資料)

經過以下公式，tail指向索引0

tail = (5 + 1) % 6

tail = 0 (指向索引0)

![img]({{site.imgurl}}/dataStruct/circular_queue2.jpg) 

### 非第1次新增

假設最大容量為maxLen = 6

原tail = 2

![img]({{site.imgurl}}/dataStruct/circular_queue3.jpg)  

tail = (2 + 1) % 6

經過以上公式，tail往後移動一位

tail = 3

![img]({{site.imgurl}}/dataStruct/circular_queue4.jpg)

把值塞入tail

![img]({{site.imgurl}}/dataStruct/circular_queue5.jpg)

### tail由最後移向第0筆

假設最大容量為maxLen = 6

若前面的資料都已被pop出去，索引0,1,2仍有空位。

tail指向最後面

原tail = 5

![img]({{site.imgurl}}/dataStruct/circular_queue6.jpg)  

tail = (5 + 1) % 6

經過以上公式，tail移到最前面索引0

tail指向陣列索引0，新增一個資料55在陣列索引0

![img]({{site.imgurl}}/dataStruct/circular_queue7.jpg)  

## pop

### pop語法

pop語法跟push語法一模一樣
{% highlight c++ linenos %}
    front_ = (front_ + 1) % maxLen;
{% endhighlight %}

把第0筆pop之前

![img]({{site.imgurl}}/dataStruct/circular_queue8.jpg)  

把第0筆pop之後

front往後移動

![img]({{site.imgurl}}/dataStruct/circular_queue9.jpg)  

## print

假設環狀佇列的狀況如下

![img]({{site.imgurl}}/dataStruct/circular_queue7.jpg)  

要把以上的值印出來

### print語法
print語法跟push與pop語法也是一模一樣，把front代入就行。

i為0 .. size實際已占用大小
{% highlight c++ linenos %}
(front_ + i) % maxLen
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

完整print語法
{% highlight c++ linenos %}
  void print() {
    for (int i = 0; i < size(); i++) {
      cout << data_[(front_ + i) % maxLen] << ",";
    }
    cout << endl;
  }
{% endhighlight %}

## 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
/**
 類別模板
 元素型別T與陣列大小maxLen是使用者設定
 */
template <class T, int maxLen>
class CircularQueue{
 public:
  // 建構子
  CircularQueue() {
    Init();  // 呼叫初始化
  }
  // 初始化函式
  void Init() {
    // 只能初始化1次，沒初始化就初始化
    if (is_init_ != true) {
      front_ = 0;  // 初始化front指向0的索引(第一個元素)
      tail_ = maxLen - 1;  // 尾指標指向陣列最後一個元素索引
      len_ = 0;  // 初始化佇列實際大小為0
      memset(data_, 0, sizeof(data_));  // 清空陣列記憶體
      is_init_ = true;  // 設成已初始化
    }
  }
  bool IsFull() {
    return len_ == maxLen;
  }
  int size() {
    return len_;
  }
  bool empty() {
    return len_ == 0;
  }
  bool push(const T &element) {
    // 判斷queue是否已經滿了
    if (IsFull()) {
      cout << "Queue is full." << endl;
      return false;
    }
    tail_ = (tail_ + 1) % maxLen;
    // 把值塞入
    data_[tail_] = element;
    len_++;
    return true;
  }
  bool pop() {
    if (empty()) return false;
    front_ = (front_ + 1) % maxLen;
    len_--;
    return true;
  }
  void print() {
    for (int i = 0; i < size(); i++) {
      cout << data_[(front_ + i) % maxLen] << ",";
    }
    cout << endl;
  }
  T& front() {
    return data_[front_];
  }
 private:
  bool is_init_;  // 是否已經初始化
  T data_[maxLen];  // 建立陣列，元素型別與陣列大小是使用者設定
  int front_;  // 前端指標，指向queue排隊最前面的元素
  int tail_;  // 尾指標，指向queue排隊最後面的元素
  int len_;  // queue實際占用大小
  // 禁止拷貝
  CircularQueue(const CircularQueue &) = delete;
  // 禁止使用等於=指派assign
  CircularQueue &operator=(const CircularQueue &) = delete;
};
int main() {
  // 資料型別為int，最大容量為6
  CircularQueue<int, 6> cir_queue;
  int tmp_val;
  // push 10,11,12
  tmp_val = 10;
  cir_queue.push(tmp_val);
  tmp_val = 11;
  cir_queue.push(tmp_val);
  tmp_val = 12;
  cir_queue.push(tmp_val);
  cout << "size = " << cir_queue.size() << endl;
  // 印出大小
  cir_queue.print();
  // pop流程
  // 取出queue中索引0
  tmp_val = cir_queue.front();
  // 移除10
  cir_queue.pop();
  cout << "pop value = " << tmp_val << endl;
  // 移除11
  tmp_val = cir_queue.front();
  cir_queue.pop();
  cout << "pop value = " << tmp_val << endl;
  // 移除12
  tmp_val = cir_queue.front();
  cir_queue.pop();
  cout << "pop value = " << tmp_val << endl;
  return 0;
}
{% endhighlight %}
