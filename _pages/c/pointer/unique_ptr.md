---
title: unique_ptr
date: 2025-03-17
keywords: c++, unique_ptr
---

unique_ptr儲存在stack中記憶體，當unique_ptr離開有效的範圍(例如:離開main函式)，就會啟動unique_ptr的解構子，解構子會去刪除unique_ptr指向的指標，就可以防止new出來的物件，但沒delete它，記憶體沒被釋放的問題。  
unique_ptr只指向一個指標，沒有辦法指向多個指標。  

## 建立

要include memory
```
#include <memory>
```
以下建立方式有註解。
{% highlight c++ linenos %}
#include <iostream>
#include <memory>
using namespace std;
class Student {
 public:
  string name_;
  Student() {
    cout << "Student 建構子" << endl;
  }
  Student(const string& name) : name_(name){
    cout << "Student一個參數 建構子" << endl;
  }
  ~Student() {
    cout << "Student 解構子" << endl;
  }
};
int main() {
  // 方式一:使用指標作為unique_ptr建構子參數
  Student* s = new Student("Mary");
  unique_ptr<Student> ptr1(s);  
  // 方式二:使用匿名物件作為unique_ptr建構子參數
  unique_ptr<Student> ptr2(new Student);
  // 方式二:使用make_unique c++14含以上才能用
  unique_ptr<Student> ptr3 = make_unique<Student>();
  // 方式二:使用make_unique一個參數的使用方法
  unique_ptr<Student> ptr4 = make_unique<Student>("Alice");
  return 0;
}
{% endhighlight %}
```
Student一個參數 建構子
Student 建構子
Student 建構子
Student一個參數 建構子
Student 解構子
Student 解構子
Student 解構子
Student 解構子
```

## 不支援assign=運算子與拷貝函式

以下會編譯失敗
{% highlight c++ linenos %}
int main() {
  Student* s = new Student("Mary");
  // 以下會編譯失敗，unique_ptr不支援拷貝函式
  unique_ptr<Student> ptr3 = s;
  unique_ptr<Student> ptr4 = new Student("Mary");
  unique_ptr<Student> ptr5 = ptr3;
  // 以下會編譯失敗，unique_ptr不支援assign運算子=
  unique_ptr<Student> ptr6;
  ptr6 = ptr3;
  return 0;
}
{% endhighlight %}

## 取值運算子
{% highlight c++ linenos %}
int main() {
  Student* s = new Student("Mary");
  unique_ptr<Student> ptr1(s);
  // 使用*取值運算子
  cout << "*s.name = " << (*s).name_ << endl;
  cout << "*ptr1.name = " << (*ptr1).name_ << endl;
  return 0;
}
{% endhighlight %}
```
Student一個參數 建構子
*s.name = Mary
*ptr1.name = Mary
Student 解構子
```

## 箭頭運算子
{% highlight c++ linenos %}
int main() {
  Student* s = new Student("Mary");
  unique_ptr<Student> ptr1(s);
  // 使用->箭頭
  cout << "s->name = " << s->name_ << endl;
  cout << "ptr1->name = " << ptr1->name_ << endl;
  return 0;
}
{% endhighlight %}
```
Student一個參數 建構子
s->name = Mary
ptr1->name = Mary
Student 解構子
```

## 記憶體位址
get()函式可以取得物件記憶體位址。
{% highlight c++ linenos %}
int main() {
  Student* s = new Student("Mary");
  unique_ptr<Student> ptr1(s);
  // 記憶體位址
  cout << "s的位址 = " << s << endl;
  cout << "ptr1 = " << ptr1 << endl;
  cout << "get() = " << ptr1.get() << endl;
  cout << "ptr1的位址 = " << &ptr1 << endl;
  return 0;
}
{% endhighlight %}
```
Student一個參數 建構子
s的位址 = 0x60000020c720
ptr1 = 0x60000020c720
get() = 0x60000020c720
ptr1的位址 = 0x7ff7bfeff2b0
Student 解構子
```

## 函式
函式參數只能參考，不能值(因為unique_ptr不支援assign=與拷貝函式)
{% highlight c++ linenos %}
void func(unique_ptr<Student> &ptr) {
  cout << "func name = " << ptr->name_ << endl;
}
int main() {
  Student* s = new Student("Mary");
  unique_ptr<Student> ptr1(s);
  // 函式參數，只能傳參考，不能傳值(因為unique_ptr不支援assign=與拷貝函式)
  func(ptr1);
  return 0;
}
{% endhighlight %}
```
Student一個參數 建構子
func name = Mary
Student 解構子
```

## 一個unique_ptr指向一個指標
{% highlight c++ linenos %}
int main() {
  Student* s = new Student("Mary");
  // 不能使用多個unique_ptr指向同一個物件，因為多個unique_ptr對同一個記憶體位址釋放多次
  // unique_ptr<Student> ptr5(s);
  // unique_ptr<Student> ptr6(s);
  // unique_ptr<Student> ptr7(s);
  return 0;
}
{% endhighlight %}

## 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <memory>
using namespace std;
class Student {
 public:
  string name_;
  Student() {
    cout << "Student 建構子" << endl;
  }
  Student(const string& name) : name_(name){
    cout << "Student一個參數 建構子" << endl;
  }
  ~Student() {
    cout << "Student 解構子" << endl;
  }
};
void func(unique_ptr<Student> &ptr) {
  cout << "func name = " << ptr->name_ << endl;
}
int main() {
  // 方式一:使用指標作為unique_ptr建構子參數
  Student* s = new Student("Mary");
  unique_ptr<Student> ptr1(s);
  // 使用*取值運算子
  cout << "*s.name = " << (*s).name_ << endl;
  cout << "*ptr1.name = " << (*ptr1).name_ << endl;
  // 使用->箭頭
  cout << "s->name = " << s->name_ << endl;
  cout << "ptr1->name = " << ptr1->name_ << endl;
  // 記憶體位址
  cout << "s的位址 = " << s << endl;
  cout << "ptr1 = " << ptr1 << endl;
  cout << "get() = " << ptr1.get() << endl;
  cout << "ptr1的位址 = " << &ptr1 << endl;
  // 函式參數，只能傳參考，不能傳值(因為unique_ptr不支援assign=與拷貝函式)
  func(ptr1);
  // 方式二:使用匿名物件作為unique_ptr建構子參數
  unique_ptr<Student> ptr2(new Student);
  // 方式二:使用make_unique c++14含以上才能用
  unique_ptr<Student> ptr3 = make_unique<Student>();
  // 方式二:使用make_unique一個參數的使用方法
  unique_ptr<Student> ptr4 = make_unique<Student>("Alice");
  // 以下會編譯失敗，unique_ptr不支援拷貝函式
  // unique_ptr<Student> ptr4 = s;
  // unique_ptr<Student> ptr4 = new Student("Mary");
  // unique_ptr<Student> ptr4 = ptr3;
  // 以下會編譯失敗，unique_ptr不支援assign運算子=
  // unique_ptr<Student> ptr4;
  // ptr4 = ptr3;
  // 不能使用多個unique_ptr指向同一個物件，因為多個unique_ptr對同一個記憶體位址釋放多次
  // unique_ptr<Student> ptr5(s);
  // unique_ptr<Student> ptr6(s);
  // unique_ptr<Student> ptr7(s);
  return 0;
}
{% endhighlight %}
