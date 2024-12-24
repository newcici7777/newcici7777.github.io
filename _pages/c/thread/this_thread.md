---
title: this_thread
date: 2024-10-22
keywords: c++, this_thread
---

## 取得get_id()

像linux中每一個行程(Process)都有pid，執行緒也有自己的id。

### 取得main thread的id

在main()函式中，放入以下程式碼就可以取得main thread的id

```
this_thread::get_id()
```


{% highlight c++ linenos %}
int main() {
  //建立執行緒t1
  thread t1(func, "test test");
  //建立執行緒t2
  thread t2(func, "abcdefg abcdefg");
  
  cout << "get_id() = " << this_thread::get_id() << endl;
  
  //執行緒t1被記憶體釋放
  t1.join();
  //執行緒t2被記憶體釋放
  t2.join();
  return 0;
}
{% endhighlight %}

### 取得thread的id

以下程式碼就可以取得執行緒的id

{% highlight c++ linenos %}
void func(string msg) {
  cout << "func get_id() = " << this_thread::get_id() << endl;
  //每一秒印1次，這裡是執行10秒
  for (int i = 0; i < 10; i++) {
    cout << "i = " << i << ", msg =" << msg << endl;
    sleep(1);//停1秒鐘
  }
}
{% endhighlight %}

### 也可以透過執行緒物件取得id

語法

```
執行緒物件.get_id()
```

程式碥

{% highlight c++ linenos %}
int main() {
  //建立執行緒t1
  thread t1(func, "test test");
  //建立執行緒t2
  thread t2(func, "abcdefg abcdefg");
  
  cout << "main() get_id() = " << this_thread::get_id() << endl;
  cout << "t1 get_id() = " << t1.get_id() << endl;
  cout << "t2 get_id() = " << t2.get_id() << endl;
  //執行緒t1被記憶體釋放
  t1.join();
  //執行緒t2被記憶體釋放
  t2.join();
  return 0;
}
{% endhighlight %}

## sleep_for ()

跟sleep()函式一樣，但sleep()會有作業系統不同造成寫法不同，匯入的標頭檔不同。但sleep_for ()解決這個問題。

以下是暫停1秒

{% highlight c++ linenos %}
this_thread::sleep_for (chrono::seconds(1));
{% endhighlight %}