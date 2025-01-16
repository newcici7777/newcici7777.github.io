---
title: semaphore
date: 2024-12-24
keywords: c++, linux, semaphore
---

Prerequisites:

- [mutex][1]
- [condition][2]

共用記憶體沒有mutex，所以使用semaphore來實現加鎖lock/解鎖unlock/等待wait/通知notify，以達到讀取相同記憶體時，搶到鎖的程序process就可以使用這個記憶體，而其它程序process就在門外排隊等待門的鎖打開，當鎖打開，就輪下一個程序process使用記憶體。

semaphore中文翻譯有太多種，訊號量(大陸)，號誌(台灣)，所以用英文代替。

## linux semaphore指令

### 列出semaphore

```
$ ipcs -s
```
------ 號誌陣列 --------
鍵值     semid      擁有者  perms      nsems     
0x00000001 0          cici       666        1      

### 刪除semaphore

在進行本頁範例前，請先手工刪除semaphore，還有[刪除共用記憶體][3]，避免產生錯誤。

```
$ ipcrm sem 0
資源已刪除
``` 
0為semid

## 初始化

### init()

{% highlight c++ linenos %}
  bool init(key_t key, unsigned short value = 1, short sem_flg = SEM_UNDO);
{% endhighlight %}

- key: 使用16進位，0x開頭，後面4個數字，例:0x0001，可跟共用記憶體的key一樣
- value: value有以下二種功能
	
	- mutex: 1
	- condition_variable: 0

- sem_flg: value有以下二種功能

	- mutex: SEM_UNDO為數字1
	- condition_variable: 0

### wait()

{% highlight c++ linenos %}
bool wait(short value = -1);
{% endhighlight %}

預設參數value = -1代表把semid減1，不是設成負1，是減1的意思。

value小於0，程式就卡在wait()這行不動，直到value大於0，執行wait()以後的程式碼

- mutex: 加鎖lock
- condition_variable: 等待wait

### post()

{% highlight c++ linenos %}
  bool post(short value = 1);
{% endhighlight %}

預設參數value = 1代表把semid加1，不是設成1，是加1的意思。

- mutex: 解鎖unlock
- condition_variable: 通知notify

檔名:semaph.h
{% highlight c++ linenos %}
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/types.h>
#include <sys/sem.h>
using namespace std;
class Semaph {
 public:
  // 初始化建構子，預設semid為-1，-1代表沒被初始化
  // 若已經初始化semid將大於等於0
  Semaph() : semid_(-1) {}
  // 初始化
  // key: 使用16進位，0x開頭，後面4個數字，例:0x0001，可跟共用記憶體的key一樣
  // value: 若semaphore沒被建立過，就建立，並把value設為1
  // value有二種功能，一種是mutex，一種是condition_variable
  bool init(key_t key, unsigned short value = 1, short sem_flg = SEM_UNDO);
  bool wait(short value = -1);
  bool post(short value = 1);
  // 取得semid
  int getvalue();
  bool destroy();
  ~Semaph();
 private:
  // union固定寫法
  union semun_ {
   int val_;
   struct semid_ds *buf_;
   unsigned short *arry_;
  };
  int semid_;
  // sem_flg是0為mutex
  // sem_flg是SEM_UNDO為condition_variable
  short sem_flg_;
  Semaph(const Semaph &) = delete;  // 禁用拷貝
  Semaph& operator=(const Semaph &) = delete;  // 禁用assign
};
{% endhighlight %}

檔名:semaph.cpp
{% highlight c++ linenos %}
#include "semaph.h"
bool Semaph::init(key_t key, unsigned short value, short sem_flg) {
  if (semid_ != -1) return false;
  sem_flg_ = sem_flg;
  if ((semid_ = semget(key, 1, 0666)) == -1) {
    if(errno == ENOENT) {
      if((semid_ = semget(key, 1, 0666 | IPC_CREAT | IPC_EXCL)) == -1) {
        if (errno == EEXIST) {
          if ((semid_ = semget(key, 1, 0666)) == -1) {
            perror("init 1 semget()");
            return false;
          }
          return true;
        } else {
          perror("init 2 semget()");
          return false;
        }
      }
      union semun_ sem_union;
      sem_union.val_ = value;
      if (semctl(semid_, 0, SETVAL, sem_union) < 0) {
        perror("init semctl()");
        return false;
      }
    } else {
      perror(" init 3 semget()");
      return false;
    }
  }
  return true;
}
bool Semaph::wait(short value) {
  if (semid_ == -1) return false;
  struct sembuf sem_b;
  sem_b.sem_num = 0;
  sem_b.sem_op = value;
  sem_b.sem_flg = sem_flg_;
  if (semop(semid_, &sem_b, 1) == -1) {
    perror("p semop()");
    return false;
  }
  return true;
}
bool Semaph::post(short value)
{
  if (semid_==-1) return false;
  struct sembuf sem_b;
  sem_b.sem_num = 0;
  sem_b.sem_op = value;
  sem_b.sem_flg = sem_flg_;
  if (semop(semid_, &sem_b, 1) == -1) {
    perror("V semop()");
    return false;
  }
  return true;
}
int Semaph::getvalue()
{
  return semctl(semid_,0,GETVAL);
}
bool Semaph::destroy()
{
  if (semid_==-1) return false;

  if (semctl(semid_, 0, IPC_RMID) == -1) {
    perror("destroy semctl()");
    return false;
  }
  return true;
}
Semaph::~Semaph(){}
{% endhighlight %}

檔名:semaph_test.cpp
{% highlight c++ linenos %}
#include "cir_queue.h"
#include "semaph.h"
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
  Semaph mutex;
  if (mutex.init(0x0001) == false) {
    cout << "mutex fail" << endl;
    return -1;
  }
  cout << "申請鎖" << endl;
  mutex.wait();
  cout << "申請鎖成功" << endl;
  strcpy(ptr->data, "abcdfrere");
  cout << "share memory data = " << ptr->data << endl;
  sleep(10);
  mutex.post();
  cout << "解鎖" << endl;
  shmdt(ptr);
}
{% endhighlight %}

編譯指令
```
g++ -o semaph_test semaph_test.cpp semaph.cpp
```

開二個終端機同時測試

一個終端機已經申請到鎖
```
$ ./semaph_test
shmid = 4
申請鎖
申請鎖成功
share memory data = abcdfrere
```

一個終端機在等待解鎖
```
$ ./semaph_test
shmid = 4
申請鎖
```

[1]: {% link _pages/c/thread/mutex.md %}
[2]: {% link _pages/c/thread/condition_variable.md %}
[3]: {% link _pages/c/libc/shared_memory.md %}#刪除共用記憶體