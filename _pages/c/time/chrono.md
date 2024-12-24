---
title: 程式執行時間
date: 2024-07-30
keywords: c++, chrono 
---

include chrono
```
#include <chrono>
```

執行的程式碼，放入要計算執行效率的程式
{% highlight c++ linenos %}
  //start 計時開始
  //end 計時結束
  std::chrono::steady_clock::time_point start,end;
  //dt 時間差
  std::chrono::nanoseconds dt;
  start = chrono::steady_clock::now(); 
  for(int i = 0;i < 100000;i++) {
    執行的程式碼
  }   
  end = chrono::steady_clock::now();
  dt = end - start;
  cout << "執行" << (double)dt.count()/(1000 * 1000 * 1000) << "秒" << endl;  
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <chrono>
using namespace std;
//參數2因為是常數字串，所以型態為const char*
char* myStrCpy(char* dest,const char* src) {
  memcpy(dest, src, strlen(src) + 1);
  return dest;
}
int main() {
  //start 計時開始
  //end 計時結束
  std::chrono::steady_clock::time_point start,end;
  //dt 時間差
  std::chrono::nanoseconds dt;
  char name[20];
  //清空陣列記憶體中的值
  memset(name, 0, sizeof(name));
  //開始計時
  start = chrono::steady_clock::now();
  for(int i = 0;i < 10000;i++) {
    myStrCpy(name, "Bill");
  }
  end = chrono::steady_clock::now();
  dt = end - start;
  cout << "執行" << (double)dt.count()/(1000 * 1000 * 1000) << "秒" << endl;
  
  cout << "name = " << name << endl;
  return 0;
}
{% endhighlight %}