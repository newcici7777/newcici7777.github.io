---
title: Shared Memory共用記憶體
date: 2024-12-20
keywords: c++, Shared Memory
---

多個進程(process)共用同一塊記憶體。

## 共用記憶體

若共用記憶體沒被建立過，則會重新建立，若已建立過，則會回傳shmid。

### include
```
#include <sys/shm.h>
```

### shmget取得共用記憶體

建立共用記憶體，沒被建立過，則會重新建立，若已建立過，則會回傳shmid

```
int shmget (key_t key, size_t size, int shmflg);
```
參數
- key鍵值 : 使用16進位，0x開頭，後面4個數字，例:0x0001
- size容量 : 結構的大小(資料物件，使用 struct；其他狀況一律使用 class)
- shmflg權限 : 參考linux權限，前面要加上0，例:0640
- shmflg權限 : IPC_CREAT代表若共用記憶體沒被建立過，則會重新建立，若已建立過，則會回傳shmid。
- 傳回值shmid : -1失敗，其它數字代表成功。

### shmat使用共用記憶體

使用共用記憶體，進程連接共用記憶體，會回傳共用記憶體位址，要把void\*轉成結構\*。

```
void* shmat(int shmid, const void *shmaddr, int shmflg);
```
參數
- shmid
- shmaddr，通常用nullptr，由系統自已設定記憶體位址
- shmflg,通常設0

### shmdt進程不再使用共用記憶體，不是刪除
```
int shmdt (const void *shmaddr);
```
- 參數傳入shmat()函式傳回的指標。
- 呼叫成功時返回一個指向共用記憶體第一個位元組的指標，如果呼叫失敗返回-1，-1要用void\*轉型。

### 程式碼

記得將以下二句轉型
<pre>
  SharedData *ptr = <span class="markline">(SharedData*)</span>shmat(shmid, nullptr, 0);
  if (ptr == <span class="markline">(void*)</span>-1) {
</pre>

結構的成員不能是string/vector/list/map(STL容器相關)，只能使用char,int,long...原生的資料型別。
<pre>
struct SharedData {
  <span class="markline">char</span> data[100];
};
</pre>

{% highlight c++ linenos %}
#include <signal.h>
#include <sys/shm.h>
#include <cstring>
using namespace std;
struct SharedData {
  char data[100];
};
int main() {
  int shmid = shmget(0x0001, sizeof(SharedData), 0640|IPC_CREAT);
  if (shmid == -1) {
    cout << "記憶體不足或權限不足，無法建立空間。" << endl;
    return -1;
  }
  cout << "shmid = " << shmid << endl;
  SharedData *ptr = (SharedData*)shmat(shmid, nullptr, 0);
  if (ptr == (void*)-1) {
    cout << "shmat() failed" << endl;
    return -1;
  }
  strcpy(ptr->data, "abcdfrere");
  cout << "share memory data = " << ptr->data << endl;
  shmdt(ptr);
}
{% endhighlight %}
```
shmid = 3
share memory data = abcdfrere
```

## 共用記憶體bash指令


### 查看共用記憶體
```
$ipcs -m
------ 共用記憶體資料段 --------
鍵值     shmid      擁有者  perms      位元組  使用數  狀
```

### 刪除共用記憶體
```
$ipcrm -m 值
```
值 = shmid

## 結合circular queue

Prerequisites:
- [circular queue][1]

檔名cir_queue.h
{% highlight c++ linenos %}
#include <iostream>
#include <cstring>
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
{% endhighlight %}

以下檔案中要注意的是轉型與呼叫init()，因為cir_que是指標，不是物件，所以要用`->`方式呼叫成員函式，而不是使用點`.`

以下程式碼是向共用記憶體insert 11,12,13，每次都只pop一個，連續執行4次，查看共用記憶體中的狀況

檔名shm_cir_que.cpp
{% highlight c++ linenos %}
#include <iostream>
#include "cir_queue.h"
#include <sys/shm.h>
#include <cstring>
using namespace std;
struct SharedData {
  char data[100];
};
int main() {
  using ElemType = int;
  // 注意轉型
  int shmid = shmget(0x0001, sizeof(CircularQueue<int, 6>), 0640|IPC_CREAT);
  if (shmid == -1) {
    cout << "記憶體不足或權限不足，無法建立空間。" << endl;
    return -1;
  }
  cout << "shmid = " << shmid << endl;
  // 注意轉型
  CircularQueue<int, 6>* cir_que = (CircularQueue<int, 6>*)shmat(shmid, nullptr, 0);
  if (cir_que == (void*)-1) {
    cout << "shmat() failed" << endl;
    return -1;
  }
  // 使用共用記憶體，不會呼叫CircularQueue的建構子，所以要手動呼叫Init()
  cir_que->Init();
  // push的流程
  ElemType tmp_val;
  tmp_val = 11;
  cir_que->push(tmp_val);
  tmp_val = 12;
  cir_que->push(tmp_val);
  tmp_val = 13;
  cir_que->push(tmp_val);
  cout << "queue size = " << cir_que->size() << endl;
  cir_que->print();
  // pop 
  tmp_val = cir_que->front();
  cir_que->pop();
  cout << "pop = " << tmp_val << endl;
  shmdt(cir_que);
}
{% endhighlight %}

```
$ g++ -o shm_cir_que shm_cir_que.cpp
$ ./shm_cir_que
```

執行結果
```
$ ./shm_cir_que
shmid = 3
queue size = 3
11,12,13,
pop = 11

$ ./shm_cir_que
shmid = 3
queue size = 5
12,13,11,12,13,
pop = 12

$ ./shm_cir_que
shmid = 3
Queue is full.
queue size = 6
13,11,12,13,11,12,
pop = 13

$ ./shm_cir_que
shmid = 3
Queue is full.
Queue is full.
queue size = 6
11,12,13,11,12,11,
pop = 11
```

[1]: {% link _pages/c/dataStruct/circular_queue.md %}