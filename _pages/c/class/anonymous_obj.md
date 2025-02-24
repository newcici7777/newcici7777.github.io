---
title: 匿名物件
date: 2024-11-28
keywords: c++, Anonymous Object
---

Prerequisites:
- [RVO][2]

匿名物件，也就是沒有名字(變數名)的物件，建立後馬上銷毀，沒有生命周期。

## 語法

```
類別名()
```

## 建立匿名物件

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  //建構子
  Student() {
    cout << "建構子" << endl;
  }
  //解構子
  ~Student() {
    cout << "解構子" << endl;
  }
};
int main() {
  //建立匿名物件
  Student();
  return 0;
}
{% endhighlight %}
```
建構子
解構子
```

## 返回匿名物件

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  //建構子
  Student() {
    cout << "建構子" << endl;
  }
  //解構子
  ~Student() {
    cout << "解構子" << endl;
  }
};
Student createStudent() {
  return Student();
}
int main() {
  createStudent();
  return 0;
}
{% endhighlight %}
```
建構子
解構子
解構子
```

程式碼執行流程
1. 呼叫 createStudent()
2. 進入 createStudent()，執行 return Student();，此時產生一個臨時對象，執行 建構子。因為你已經關閉 RVO（Return Value Optimization），這個臨時物件無法被最佳化，因此會觸發額外的拷貝函式，拷貝完之後進行銷毀臨時物件。
3. main() 中沒有其他 Student 物件，因此程式結束，銷毀createStudent()的返回拷貝函式產生的物件。


## 匿名物件指派給變數
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  //建構子
  Student() {
    cout << "建構子" << endl;
  }
  //解構子
  ~Student() {
    cout << "解構子" << endl;
  }
};
int main() {
  Student s1 = Student();
  return 0;
}
{% endhighlight %}
```
建構子
解構子
解構子
```

程式碼執行流程
1. 呼叫 createStudent()
2. 進入 createStudent()，執行 return Student();，此時產生一個臨時對象，執行 建構子。因為你已經關閉 RVO（Return Value Optimization），這個臨時物件無法被最佳化，因此會觸發額外的拷貝函式，拷貝完之後進行銷毀臨時物件。
3. 程式結束，銷毀s1物件。

## 匿名物件也是臨時物件

匿名物件是直接由程式碼直接生成。

{% highlight c++ linenos %}
int main() {
  //由程式碼直接建立物件
  Student();
  return 0;
}
{% endhighlight %}

若函式返回的是匿名物件，編譯器自動管理其生命週期，就會變成臨時物件，所謂的臨時物件就是由編譯器生成。

通過函式傳回的物件(包含匿名物件或不是匿名物件)，全是臨時物件。


## initializer_list匿名物件
- [initializer list][1]

以下程式碼由initializer_list方式建立物件

\{8, \"cici\"\}
與
\{3, \"john\"\}
為匿名物件，因為他們沒有名字。

{% highlight c++ linenos %}
#include <map>
#include <string>
using namespace std;
int main() {
  map<int, string> m;
  m.insert({ {8, "cici"}, {3, "john"} });
  return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/c11/init_list.md %}
[2]: {% link _pages/c/editor/rvo.md %}