---
title: condition_variable
date: 2024-10-23
keywords: c++, condition_variable, notify, wait
---

## 通知與等待

想像一下，當有人寄簡訊給你，你的手機會收到簡訊的通知Notify，其它時候，你的手機一直處於等待Wait的狀態。

![img]({{site.imgurl}}/dataStruct/msg.jpg) 

### 建立condition_variable

```
condition_variable m_cond;//條件
```

### 通知接收訊息

通知一個執行緒
```
m_cond.notify_one();
```

通知多個執行緒
```
m_cond.notify_all();
```

### unique_lock

unique_lock支援condition_variable(通知/等待)

unique_lock支援解鎖功能。

{% highlight c++ linenos %}
unique_lock<mutex> lock(m_mtx);//加鎖

lock.unlock();//解鎖
{% endhighlight %}

也可使用scope有效範圍作為離開範圍解鎖
{% highlight c++ linenos %}
{
	unique_lock<mutex> lock(m_mtx);//加鎖
	//離開scope{}就解鎖
}
{% endhighlight %}

### 等待有訊息傳過來

{% highlight c++ linenos %}
m_cond.wait(lock);
{% endhighlight %}

## Queue

建立一個Queue儲存訊息。

## 無限迴圈

手機無限迴圈等待有訊息通知


## 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
#include <thread>
#include <mutex>
#include <queue>
#include <condition_variable>
using namespace std;
class SafeQueue {
  mutex m_mtx;//執行緒
  condition_variable m_cond;//條件
  queue<string> m_que;
public:
  void push(string& msg) {
  //加鎖，scope的範圍為函式內
  lock_guard<mutex> lock(m_mtx);
  m_que.push(msg);//放入訊息
  m_cond.notify_one();//通知執行緒來接收訊息
  }
  void pop(){
  //無限迴圈等待有訊息通知
  while (true) {
    //每個執行緒有獨立的msg變數
    string msg;
    //加鎖
    //m_cond.wait()參數只支援unique_lock
    unique_lock<mutex> lock(m_mtx);
    // queue是空的才等待
    while (m_que.empty()) {
    //queue沒資料就等待
    m_cond.wait(lock);
    }
    //被通知接收資料
    //若queue有資料了，才做下面的事情
    msg = m_que.front();//拿出第一個元素
    m_que.pop();//移除元素
    //印出接收到的訊息
    cout << "執行緒:" << this_thread::get_id() << "," << msg << endl;
    //解鎖
    lock.unlock();
    //如果msg是end就不要再等待接收訊息
    if (msg == "END") break;
  }
  }
};
int main() {
  //建立物件
  SafeQueue safeQue;
  //建立3個執行緒
  thread t1(&SafeQueue::pop, &safeQue);
  thread t2(&SafeQueue::pop, &safeQue);
  thread t3(&SafeQueue::pop, &safeQue);
  //產生100個訊息
  for (int i = 0; i < 100; i++) {
  string temp_msg = "msg" + to_string(i);
  safeQue.push(temp_msg);
  }
  //產生結束訊息，跳離無限迴圈，不要再等待接收訊息
  for (int i = 0; i < 3; i++) {
  string end_msg = "END";
  safeQue.push(end_msg);
  }
  //執行緒記憶體釋放
  t1.join();
  t2.join();
  t3.join();
  return 0;
}
{% endhighlight %}

```
執行緒:0x70000ff3d000,msg0
執行緒:0x70000ff3d000,msg1
執行緒:0x70000ff3d000,msg2
執行緒:0x70000ff3d000,msg3
執行緒:0x70000ff3d000,msg4
執行緒:0x70000ff3d000,msg5
執行緒:0x70000ff3d000,msg6
執行緒:0x70000ff3d000,msg7
執行緒:0x70000ff3d000,msg8
執行緒:0x70000ff3d000,msg9
執行緒:0x70000ff3d000,msg10
執行緒:0x70000ff3d000,msg11
執行緒:0x70000ffc0000,msg12
執行緒:0x70000ffc0000,msg13
執行緒:0x70000ffc0000,msg14
執行緒:0x70000ffc0000,msg15
以下截掉
```

## 證明wait()自帶解鎖功能

因為mutex互斥鎖一次只能把鎖給一個執行緒，拿到鎖的執行緒就可以執行互斥鎖mutex之後的程式碼，其它執行緒都在mutex之外排隊，
但wait()的參數中又需要有lock，只能有鎖的執行緒才能等待通知，這樣就只有一個執行緒在等待通知，
其它執行緒都在mutex之外排隊等待拿到鎖，因此wait()為了要讓其它執行緒能等待接收訊息，自帶解鎖功能，證明的過程在最後面。

在wait()之前增加一行，休眠一小時
```
this_thread::sleep_for (chrono::hours(1));
```
目的是不要執行wait()，可以看到執行緒搶鎖的過程

{% highlight c++ linenos %}
void pop(){
  //無限迴圈等待有訊息通知
  while (true) {
    //每個執行緒有獨立的msg變數
    string msg;
    //加鎖
    //m_cond.wait()參數只支援unique_lock
    cout << "執行緒 = " << this_thread::get_id() << "排隊等待" << endl;
    unique_lock<mutex> lock(m_mtx);
    cout << "執行緒 = " << this_thread::get_id() << "加鎖成功" << endl;
    this_thread::sleep_for (chrono::hours(1));
    // queue是空的才等待
    while (m_que.empty()) {
    //queue沒資料就等待
    m_cond.wait(lock);
    }
    //被通知接收資料
    //若queue有資料了，才做下面的事情
    msg = m_que.front();//拿出第一個元素
    m_que.pop();//移除元素
    //印出接收到的訊息
    cout << "執行緒:" << this_thread::get_id() << "," << msg << endl;
    //解鎖
    lock.unlock();
    //如果msg是end就不要再等待接收訊息
    if (msg == "END") break;
  }
  }
};
{% endhighlight %}

從執行結果可以看出，只有一個執行緒拿到鎖，並加鎖，其它執行緒都在外面排隊等待拿到鎖。

```
執行緒 = 執行緒 = 0x7000016b9000執行緒 = 0x700001636000排隊等待0x70000173c000排隊等待
排隊等待

執行緒 = 0x7000016b9000加鎖成功
```

把休眠一小時的程式碼拿掉

{% highlight c++ linenos %}
void pop(){
  //無限迴圈等待有訊息通知
  while (true) {
    //每個執行緒有獨立的msg變數
    string msg;
    //加鎖
    //m_cond.wait()參數只支援unique_lock
    cout << "執行緒 = " << this_thread::get_id() << "排隊等待" << endl;
    unique_lock<mutex> lock(m_mtx);
    cout << "執行緒 = " << this_thread::get_id() << "加鎖成功" << endl;
    // queue是空的才等待
    while (m_que.empty()) {
    //queue沒資料就等待
    m_cond.wait(lock);
    }
    //被通知接收資料
    //若queue有資料了，才做下面的事情
    msg = m_que.front();//拿出第一個元素
    m_que.pop();//移除元素
    //印出接收到的訊息
    cout << "執行緒:" << this_thread::get_id() << "," << msg << endl;
    //解鎖
    lock.unlock();
    //如果msg是end就不要再等待接收訊息
    if (msg == "END") break;
  }
  }
};
{% endhighlight %}

從執行結果可以看到，三個執行緒都拿到鎖，證明wait()有自帶解鎖功能。

```
執行緒 = 執行緒 = 執行緒 = 0x70000ea07000排隊等待0x70000ea8a000
執行緒 = 排隊等待
0x70000ea07000加鎖成功
執行緒 = 0x70000ea8a000加鎖成功
0x70000e984000排隊等待
執行緒 = 0x70000e984000加鎖成功
```

