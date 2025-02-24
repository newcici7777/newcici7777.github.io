---
title: operator=()
date: 2024-10-28
keywords: c++, operator=
---

Prerequisites:
- [RVO][1]

## 指派運算子使用方式

二個物件都`已經存在`，其中一個物件指派另一個物件。

編譯器預設會有預設的指派運算子operator=()，使用者可改寫。

改寫operator=函式的語法如下

```
類別名& operator=(const 類別名& src來源物件);
```

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string m_name;
  Student& operator=(const Student& s) {
    m_name = s.m_name;
    return *this;
  }
};
int main() {
  //建立二個物件
  Student s1,s2;
  s1.m_name = "Bill";
  
  //使用operator=函式
  s2 = s1;
  cout << "s1 name = " << s1.m_name << endl;
  cout << "s2 name = " << s2.m_name << endl;
  return 0;
}
{% endhighlight %}

```
s1 name = Bill
s2 name = Bill
```

## 深拷貝

預設是淺拷貝。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string m_name;
  int* m_ptr;
  Student(){m_ptr = nullptr;}
  ~Student(){if(m_ptr) delete m_ptr;}
  //參數為來源物件，也就是指要copy的物件
  Student& operator=(const Student& src) {
    m_name = src.m_name;
    //如果來源物件的m_ptr指標是空
    if(src.m_ptr == nullptr) {
      //要把目的物件清空(如果目的物件不是空的話)
      if(m_ptr != nullptr) {
        delete m_ptr;
        m_ptr = nullptr;
      }
    } else {
      //如果目的物件為空
      if(m_ptr == nullptr) {
        //動態分配記憶體
        m_ptr = new int;
      }
      //記憶體的值拷貝
      memcpy(m_ptr, src.m_ptr, sizeof(int));
    }
    return *this;
  }
};
int main() {
  //建立二個物件
  Student s1,s2;
  s1.m_name = "Bill";
  s1.m_ptr = new int(15);
  //使用operator=函式
  s2 = s1;
  cout << "s1 name = " << s1.m_name << endl;
  cout << "s2 name = " << s2.m_name << endl;
  cout << "s1 m_ptr = " << *s1.m_ptr << endl;
  cout << "s2 m_ptr = " << *s2.m_ptr << endl;
  return 0;
}
{% endhighlight %}
```
s1 name = Bill
s2 name = Bill
s1 m_ptr = 15
s2 m_ptr = 15
```

[1]: {% link _pages/c/editor/rvo.md %}