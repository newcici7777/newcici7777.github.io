---
title: 多型
date: 2026-03-21
keywords: python, polymorphism
---
Prerequisites:

- [C++ 多型][1]
- [Java 多型][2]

Python的多型，在成員變數的部分，跟C++與Java不一樣。<br>

## C++ 多型
Parent與Child都有相同成員變數name，但子類別沒有覆寫Parent的hi()函式，執行結果為Parent的name。<br>
{% highlight c++ linenos %}
#include <iostream>
#include "main.h"
using namespace std;
class Parent {
 public:
  string name = "Parent";
  virtual void hi() {
    cout << "name = " << this->name << endl;
  }
};
class Child:public Parent {
 public:
  string name = "Child";
};
int main() {
  Parent* ptr = new Child;
  ptr->hi();
  return 0;
}
{% endhighlight %}
```
Parent
```

## Java 多型
Parent與Child都有相同成員變數name，但子類別沒有覆寫Parent的hi()函式，執行結果為Parent的name。<br>
{% highlight python linenos %}
public class Test {
  public static void main(String[] args) {
    Parent obj = new Child();
    obj.hi();
  }
}
class Parent {
  String name = "Parent";
  public void hi() {
    System.out.println("name = " + this.name);
  }
}
class Child extends Parent{
  String name = "Child";
}
{% endhighlight %}
```
name = Parent
```

## Python 多型
Parent與Child都有相同成員變數name，但子類別沒有覆寫Parent的hi()函式，執行結果為Child的name。<br>
{% highlight python linenos %}
class Parent:
    name = "Father"

    def hi(self):
        print("name =", self.name)

class Child(Parent):
    name = "Child"

obj = Child()
obj.hi()
{% endhighlight %}
```
name = Child
```

## Python vs Java vs C++
Python在讀取 self.name 成員變數，先從子類別Child找，有這個屬性就取得。<br>
如果子類別沒有name屬性，就去父類別找，如果仍找不到就去祖父類別找，直到找到為止。<br>

Java 與 C++:<br>
執行誰的方法，就是使用誰的成員變數。<br>
執行父類別的方法，使用父類別的成員變數。<br>
執行子類別的方法，使用子類別的成員變數。<br>

因為Child類別沒有hi()方法，就會去執行父類別的hi()方法，就會使用父類別的name成員變數。<br>

[1]: {% link _pages/c/class/polymorphism.md %}
[2]: {% link _pages/java/polymorphism.md %}
