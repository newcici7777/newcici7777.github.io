---
title: callback
date: 2024-11-8
keywords: c++, callback
---
Prerequisites:
- [condition_variable][1]
- [可變參數模板與bind與functional][2]

## 增加callback

在condition_variable的程式碼中加上callback的程式碼

主要程式碼如下

{% highlight c++ linenos %}
class SafeQueue {
    //成員變數為函式，函式類型為void(const string& msg)
    function<void(const string& msg)> m_callback;
public:
	//成員函式模板
	//參數1函式，參數2物件(可能沒有物件所以用可變參數Args...)
    template<typename Func, typename... Args>
    void callback(Func&& func, Args&&... args) {    
    	//args是物件，若呼叫的函式是物件成員函式，第二個參數就是物件
    	//若是函式，就不需要傳入第2個參數(因為可能沒有第2個參數，所以用可變參數Args...)
        m_callback = bind(forward<Func>(func), forward<Args>(args)..., std::placeholders::_1);
    }

    .... 以下程式碼略過

    //若有設定回呼函式
    if(m_callback)
    	//執行回呼函式，並把回呼函式的參數傳入 
    	m_callback(msg);

    .... 以下程式碼略過

{% endhighlight %}

## 完整程式碼

{% highlight c++ linenos %}
class SafeQueue {
    mutex m_mtx;//執行緒
    condition_variable m_cond;//條件
    queue<string> m_que;
    //成員變數為函式，函式類型為void(const string& msg)
    function<void(const string& msg)> m_callback;
public:
	//成員函式模板
	//參數1函式，參數2物件(可能沒有物件所以用可變參數Args...)
    template<typename Func, typename... Args>
    void callback(Func&& func, Args&&... args) {    
    	//args是物件，若呼叫的函式是物件成員函式，第二個參數就是物件
    	//若是函式，就不需要傳入第2個參數(因為可能沒有第2個參數，所以用可變參數Args...)
        m_callback = bind(forward<Func>(func), forward<Args>(args)..., std::placeholders::_1);
    }
    void push(string& msg) {
        //加鎖，scope的範圍為函式內
        lock_guard<mutex> lock(m_mtx);
        m_que.push(msg);//放入訊息
        m_cond.notify_one();//通知執行緒來接收訊息
    }
    void pop(){
        //無限迴圈等待有訊息通知
        while(true) {
            //每個執行緒有獨立的msg變數
            string msg;
            //加鎖
            //因為m_cond.wait()參數只支援unique_lock
            unique_lock<mutex> lock(m_mtx);
            // queue是空的才等待
            while(m_que.empty()) {
                //queue沒資料就等待
                m_cond.wait(lock);
            }
            //被通知接收資料
            //若queue有資料了，才做下面的事情
            msg = m_que.front();//拿出第一個元素
            m_que.pop();//移除元素
            //解鎖
            lock.unlock();
            //若有設定回呼函式
            if(m_callback)
            	//執行回呼函式，並把回呼函式的參數傳入 
            	m_callback(msg);

            //如果msg是end就不要再等待接收訊息
            if(msg == "END") break;
        }
    }
};
{% endhighlight %}

## callback是成員函式

{% highlight c++ linenos %}
    Student student;
    //參數為物件成員函式，第二個參數是物件
    safeQue.callback(&Student::print, student);
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
void print(const string& msg) {
    cout << " Msg = " << msg << endl;
}
class SafeQueue {
    mutex m_mtx;//執行緒
    condition_variable m_cond;//條件
    queue<string> m_que;
    function<void(const string& msg)> m_callback;
public:
    template<typename Func, typename... Args>
    void callback(Func&& func, Args&&... args) {
        m_callback = bind(forward<Func>(func), forward<Args>(args)..., std::placeholders::_1);
    }
    void push(string& msg) {
        //加鎖，scope的範圍為函式內
        lock_guard<mutex> lock(m_mtx);
        m_que.push(msg);//放入訊息
        m_cond.notify_one();//通知執行緒來接收訊息
    }
    void pop(){
        //無限迴圈等待有訊息通知
        while(true) {
            //每個執行緒有獨立的msg變數
            string msg;
            //加鎖
            //因為m_cond.wait()參數只支援unique_lock
            unique_lock<mutex> lock(m_mtx);
            // queue是空的才等待
            while(m_que.empty()) {
                //queue沒資料就等待
                m_cond.wait(lock);
            }
            //被通知接收資料
            //若queue有資料了，才做下面的事情
            msg = m_que.front();//拿出第一個元素
            m_que.pop();//移除元素
            //解鎖
            lock.unlock();
            if(m_callback) m_callback(msg);
            //如果msg是end就不要再等待接收訊息
            if(msg == "END") break;
        }
    }
};
int main() {
    //建立物件
    SafeQueue safeQue;
    //建立物件
    Student student;
    //參數為物件成員函式，第二個參數是物件
    safeQue.callback(&Student::print, student);
    //建立3個執行緒
    thread t1(&SafeQueue::pop, &safeQue);
    thread t2(&SafeQueue::pop, &safeQue);
    thread t3(&SafeQueue::pop, &safeQue);
    //產生100個訊息
    for(int i = 0; i < 100; i++) {
        string temp_msg = "msg" + to_string(i);
        safeQue.push(temp_msg);
    }
    //產生結束訊息，跳離無限迴圈，不要再等待接收訊息
    for(int i = 0; i < 3; i++) {
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
Msg = msg0
 Msg =  Msg = msg2
 Msg =  Msg = msg4
msg1
 Msg =  Msg = msg3msg6msg5


 Msg = msg7 Msg = msg9
 Msg = 
 .
 .
 .
 以下截掉
 ```
## callback參數為函式

以下新增print()函式
callback函式設為print

```
    safeQue.callback(print);
```

{% highlight c++ linenos %}
void print(const string& msg) {
    cout << " Msg = " << msg << endl;
}
class SafeQueue {
 .
 .
 .
 以下截掉(跟前一個程式碼一模一樣)
}
int main() {
    //建立物件
    SafeQueue safeQue;
    //設定回呼函式
    safeQue.callback(print);
    //建立3個執行緒
    thread t1(&SafeQueue::pop, &safeQue);
    thread t2(&SafeQueue::pop, &safeQue);
    thread t3(&SafeQueue::pop, &safeQue);
    //產生100個訊息
    for(int i = 0; i < 100; i++) {
        string temp_msg = "msg" + to_string(i);
        safeQue.push(temp_msg);
    }
    //產生結束訊息，跳離無限迴圈，不要再等待接收訊息
    for(int i = 0; i < 3; i++) {
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



[1]: {% link _pages/c/thread/condition_variable.md %}
[2]: {% link _pages/c/c11/variadic.md %}#可變參數模板與bind與functional