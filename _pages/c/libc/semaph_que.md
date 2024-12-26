---
title: semaphore作為condition
date: 2024-12-24
keywords: c++, linux, semaphore
---
Prerequisites:

- [mutex][1]
- [condition][2]
- [semaphore][3]
- [circular_queue][4]
- [結合circular queue][5]

延續先前semaphore與mutex的範例，這頁是semaphore作為condition的範例，使用circular queue作為範例

檔名:push_test.cpp
{% highlight c++ linenos %}
#include <iostream>
#include "semaph.h"
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
  // 建立mutex
  Semaph mutex;
  mutex.init(0x0001);
  // 建立condition
  Semaph condition;
  condition.init(0x0002, 0, 0);
  // lock加鎖
  mutex.wait();
  // push的流程
  ElemType tmp_val;
  tmp_val = 11;
  cir_que->push(tmp_val);
  tmp_val = 12;
  cir_que->push(tmp_val);
  tmp_val = 13;
  cir_que->push(tmp_val);
  // unlock解鎖
  mutex.post();
  // 計數功能
  condition.post(3);  // 參數3代表生產3個資料
  cir_que->print();
  shmdt(cir_que);
}
{% endhighlight %}

檔名:pop_test.cpp
{% highlight c++ linenos %}
#include <iostream>
#include "semaph.h"
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
  // 建立mutex
  Semaph mutex;
  mutex.init(0x0001);
  // 建立condition
  Semaph condition;
  condition.init(0x0002, 0, 0);
  // 存放pop出來的資料
  ElemType tmp_val;
  while (true) {
    // lock加鎖
    mutex.wait();
    while (cir_que->empty()) {  // 若佇列為空，無限迴圈
      mutex.post();  // unlock解鎖
      condition.wait();  // 等待有資料，沒資料就一直卡在這
      // 若有資料就會執行以下這行，並離開cir_que->empty的迴圈
      mutex.wait();  // lock加鎖
    }
    // pop資料
    tmp_val = cir_que->front();
    cout << "tmp_val = " << tmp_val << endl;
    cir_que->pop();
    // unlock解鎖
    mutex.post();
    cout << "val = " << tmp_val << endl;
    // 假設處理資料要花10秒鐘
    sleep(10);
  }
  shmdt(cir_que);
}
{% endhighlight %}

編譯指令
```
g++ -o push_test push_test.cpp semaph.cpp
g++ -o pop_test pop_test.cpp semaph.cpp
```

打開二個終端機視窗

一個執行push
```
./push_test
```

一個執行pop
```
./pop_test
```

執行結果

push
```
$ ./push_test
shmid = 5
Queue is full.
Queue is full.
Queue is full.
11,12,13,11,12,13,

$ ./push_test
shmid = 5
11,12,13,
```

pop
```
$ ./pop_test
shmid = 5
tmp_val = 11
val = 11
tmp_val = 12
val = 12
tmp_val = 13
val = 13
tmp_val = 11
val = 11
tmp_val = 12
val = 12
tmp_val = 13
val = 13
tmp_val = 11
val = 11
tmp_val = 12
val = 12
tmp_val = 13
val = 13
^C
```

[1]: {% link _pages/c/thread/mutex.md %}
[2]: {% link _pages/c/thread/condition_variable.md %}
[3]: {% link _pages/c/libc/shared_memory.md %}#刪除共用記憶體
[4]: {% link _pages/c/dataStruct/circular_queue.md %}
[5]: {% link _pages/c/libc/shared_memory.md %}#結合circular-queue