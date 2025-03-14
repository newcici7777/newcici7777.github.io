---
title: 繼承初始化
date: 2025-03-01
keywords: c++, inherit init
---
Prerequisites:
- [建構子初始化][1]

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  int public_member_;
 private:
  int private_member_;
 public:
  Parent() : public_member_(0), private_member_(0) {
    cout << "父類別建構子" << endl;
  }
  Parent(int param1, int param2)
    : public_member_(param1),
      private_member_(param2) {
        cout << "父類別建構子" << endl;
  }
  // 父類別拷貝函式，將傳來的物件中的值assign給二個成員變數
  Parent(const Parent &p)
    : public_member_(p.public_member_),
      private_member_(p.private_member_) {
    cout << "父類別拷貝建構子" << endl;
  }
  void showParent() {
    cout << "父類別 public_member_ = " << public_member_ << ",private_member_ = " << private_member_ << endl;
  }
};
class Child : public Parent {
 public:
  int child_member_;
  // 子類別預設會呼叫父類別的空建構子，就算省略不寫:Parent()，編譯器也會自動產生
  // 並初始化child_member_值為100
  Child() : Parent(), child_member_(100) {}
  Child(int param1, int param2, int param3)
    : Parent(param1, param2), child_member_(param3) {
      cout << "子類別3個參數建構子" << endl;
  }
  // 呼叫父類別的拷貝建構子
  Child(const Parent& parent, int param3)
    : Parent(parent), child_member_(param3) {
      cout << "子類別2個參數建構子" << endl;
  }
  void showChild() {
    cout << "child_member = " << child_member_ << endl;
  }
};
int main() {
  // 呼叫子類別空建構子
  Child child1;
  child1.showChild();
  // 呼叫子類別3個參數建構子
  Child child2(5, 10, 15);
  child2.showChild();
  // 呼叫父類別2個參數建構子
  Parent p1(22, 33);
  //子類別2個參數的建構子，並且呼叫父類別拷貝函式
  Child child3(p1, 44);
  // 呼叫父類別的函式
  child3.showParent();
  child3.showChild();
  return 0;
}
{% endhighlight %}

```
父類別建構子
child_member = 100
父類別建構子
子類別3個參數建構子
child_member = 15
父類別建構子
父類別拷貝建構子
子類別2個參數建構子
父類別 public_member_ = 22,private_member_ = 33
child_member = 44
```
[1]: {% link _pages/c/class/init_list.md %}