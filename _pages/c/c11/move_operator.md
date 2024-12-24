---
title: 移動建構子與移動指派運算子
date: 2024-11-4
keywords: c++, Move assignment operator 
---

Prerequisites:

- [拷貝函式][1]
- [operator=()][2]
- [深淺拷貝][3]
- [l-value與r-value][4]
- [l-value參考與r-value參考][5]
- [臨時物件][6]
- [關閉RVO][7]

把RVO關閉後，才能進行以下的範例。

## 目的

以下的拷貝函式，針對指標(m_ptr)動態分配記憶體位址，將來源(source)物件指標內容拷貝到新分配的記憶體位址。
{% highlight c++ linenos %}
  Student(const Student &s) {
    //判斷來源(source)m_ptr是否null
    if(s.m_ptr) {
    	//動態分配記憶體位址
      m_ptr = new int;
      //拷貝來源物件m_ptr指標到新的記憶體位址
      memcpy(m_ptr, src.m_ptr, sizeof(int));
    } else {
      m_ptr = nullptr;
    }
  }
{% endhighlight %}

以上程式碼若拷貝的容量很大，將會花費許多時間在拷貝資料。

移動建構子(Move constructor)與移動指派運算子(Move assignment operator)主要針對右值進行指標移動，因為右值是一個臨時物件，通常使用完畢立即記憶體釋放，移動建構子與移動指派運算子是不釋放臨時物件記憶體，把指標指向臨時物件(來源物件)的記憶體位址，省略以上程式碼拷貝動作。

## 語法

移動建構子(Move constructor)與移動指派運算子(Move assignment operator)的參數都是右值(r-value)，因為要刪除來源物件指標(有修改的動作)，所以參數不用const，拷貝與指派運算子則是不會修改來源物件。

### 移動建構子(Move constructor)

```
類別名(類別名&& 來源物件) {
	....
}
```

### 移動指派運算子(Move assignment operator)

```
類別名& operator=(類別名&& 來源物件) {
	......
	return *this;
}
```

### 指標轉移語法

{% highlight c++ linenos %}
  Student(Student&& src) {
    cout << "移動建構子" << endl;
    //把來源的指標移到m_ptr
    m_ptr = src.m_ptr;
    //把來源的指標設nullptr
    src.m_ptr = nullptr;
  }
  Student& operator=(Student&& src) {
    cout << "移動指派運算子" << endl;
    //把來源的指標移到m_ptr
    m_ptr = src.m_ptr;
    //把來源的指標設nullptr
    src.m_ptr = nullptr;
    return *this;
  }
{% endhighlight %}

### 使用右值移動建構子

第一次右值移動建構子：return s;這個臨時對象被移動到一個中間的無名臨時對象中，觸發了第一次的右值移動建構子。

第二次右值移動建構子：接著，這個中間的無名臨時對象被移動到 s4 中，觸發了第二次的右值移動建構子。

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
class Student {
public:
  int* m_ptr;
public:
  Student() {
    cout << "建構子" << endl;
    //初始化成員變數
    m_ptr = nullptr;
  }
  Student(const Student &src) {
    cout << "左值拷貝函式" << endl;
    //判斷來源(source)m_ptr不是null
    if(src.m_ptr) {
      //動態分配記憶體位址
      m_ptr = new int;
      //拷貝來源物件m_ptr指標到新的記憶體位址
      memcpy(m_ptr, src.m_ptr, sizeof(int));
    } else {
      m_ptr = nullptr;
    }
  }
  Student& operator=(const Student& src) {
    cout << "左值指派運算子" << endl;
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
        memset(m_ptr, 0, sizeof(int));
      }
      //記憶體的值拷貝
      memcpy(m_ptr, src.m_ptr, sizeof(int));
    }
    return *this;
  }
  Student(Student&& src) {
    cout << "右值移動建構子" << endl;
    //把來源的指標移到m_ptr
    m_ptr = src.m_ptr;
    //把來源的指標設nullptr
    src.m_ptr = nullptr;
  }
  Student& operator=(Student&& src) {
    cout << "右值移動指派運算子" << endl;
    //把來源的指標移到m_ptr
    m_ptr = src.m_ptr;
    //把來源的指標設nullptr
    src.m_ptr = nullptr;
    return *this;
  }
  ~Student() {
    delete m_ptr;
    m_ptr = nullptr;
    cout << "解構子" << endl;
  }
  void print() {
    //印出記憶體位址
    cout << "m_ptr address:" << &m_ptr << endl;
    cout << "m_ptr value = " << *m_ptr << endl;
  }
};
int main() {
  //右值指派運算子
  Student s4 = [] {
    Student s;
    s.m_ptr = new int;
    *s.m_ptr = 200;
    return s;
  }();
  cout << "s4 mptr = " << *s4.m_ptr << endl;
  return 0;
}
{% endhighlight %}


```
建構子
右值移動建構子
解構子
右值移動建構子
解構子
s4 mptr = 200
解構子
```



[1]: {% link _pages/c/class/copy_constructor.md %}
[2]: {% link _pages/c/class/operator_assign.md %}
[3]: {% link _pages/c/class/deep_shallow_copy.md %}
[4]: {% link _pages/c/c11/rvalue/l_r_value.md %}
[5]: {% link _pages/c/c11/rvalue/l_r_ref.md %}
[6]: {% link _pages/c/class/temp_obj.md%}
[7]: {% link _pages/c/editor/rvo.md %}