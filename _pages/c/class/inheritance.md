---
title: 繼承
date: 2025-02-24
keywords: c++, inheritance
---

## 子類繼承父類
子類繼承父類，子類可以使用父類的成員變數與成員函式。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  string name_;
  string tel_;
  void setName(const string& name) {
    name_ = name;
  }
  void setTel(const string& tel) {
    tel_ = tel;
  }
};
class Child:public Parent {
 public:
  int id_;
  void setId(const int id) {
    id_ = id;
  }
  void print() {
    cout << "id = " << id_ << ", name = " << name_ << ", tel = " << tel_ << endl;
  }
};
int main() {
  // 建立child物件
  Child child;
  // 使用父類的setName成員函式
  child.setName("Cici");
  // 使用父類的setTel成員函式
  child.setTel("08xxxxxx");
  child.setId(100);
  child.print();
  return 0;
}
{% endhighlight %}

## 繼承方式

{% highlight c++ linenos %}
class Child1:public Parent {
};
{% endhighlight %}

以上的:public，public就是繼承方式

繼承方式有三種，public、protected、private。

若繼承方式是protected，父類的public成員函式與成員變數會降級成protected，影嚮到孫子類存取祖父類的成員函式與變數。

若繼承方式是private，父類的public成員函式與成員變數會降級成private，影嚮到孫子類無法存取祖父類的成員函式與變數。

|繼承方式	|在 main()存取父類public成員函式|在子類存取父類public成員函式|在孫子類存取祖父類public成員函式|
|public	|可存取|	可存取| 可存取|
|protected|無法存取|可存取|可存取|
|private|無法存取	|可存取|無法存取|

以下程式碼透過public的printId()與setId()就可以由外部main()函式去寫入私有成員變數與讀取私有成員變數。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  string name_;
  Parent(){}
  void setId(const string& id) {
    id_ = id;
  }
  void printId() {
    cout << "id = " << id_ << endl;
  }
 private:
  string id_;
};
{% endhighlight %}


{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  string name_;
  Parent(){}
  void setName(const string& name) {
    name_ = name;
  }
  void setTel(const string& tel) {
    tel_ = tel;
  }
  void setId(const string& id) {
    id_ = id;
  }
  void printId() {
    cout << "id = " << id_ << endl;
  }
 protected:
  string tel_;
 private:
  string id_;
};
class Child1:public Parent {
 public:
  void print() {
    // 父類id_成員變數是private無法使用
    cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
  }
};
//繼承方式為protected，父類的public成員變數與函式全變成protected
class Child2:protected Parent {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 父類id_成員變數是private無法使用
   // 父類的成員函式降級protected，但仍能在子類成員函式使用
   // 無法在main()外部使用
   setName(name);
   setTel(tel);
   setId(id);
   printId();
   // 父類的成員變數的仍可給child成員函式使用
   cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
 }
};
class GrandChild2:protected Child2 {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 祖父id_成員變數是private無法使用
   // 祖父的成員函式降級protected，但仍能在孫子類成員函式使用
   // 無法在main()外部使用
   setName(name);
   setTel(tel);
   setId(id);
   printId();
   // 父類的成員變數的仍可給child成員函式使用
   cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
 }
};
class Child3:private Parent {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 父類id_成員變數是private無法使用
   // 父類的成員函式降級private無法使用
   setName(name);
   setTel(tel);
   setId(id);
   printId();
   // 父類的成員變數的仍可給child成員函式使用
   cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
 }
};
class GrandChild3:private Child3 {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 祖父類id_成員變數是private無法使用
   // 祖父類的成員函式降級private無法使用
   //setName(name);
   //setTel(tel);
   //setId(id);
   //printId();
   // 祖父類的成員變數降級private無法使用
 }
};
int main() {
  // 建立child物件
  Child1 child1;
  child1.setName("Cici");
  child1.setTel("080xxxx");
  child1.print();
  Child2 child2;
  // 因為繼承方式是protected，所以父類的public成員函式全降級變為protected成員函式
  // protected就不能在main()外部使用
  //child2.setName("Cici2");
  //child2.setTel("080xxxx2");
  child2.print("Bill","09xxxx","0001");
  GrandChild2 gchild2;
  gchild2.print("gBill","g09xxxx","g0001");
  Child3 child3;
  child3.print("Jerry","08xxxx","0002");
  GrandChild3 gchild3;
  gchild3.print("gJerry","g08xxxx","g0002");
  return 0;
}
{% endhighlight %}
```
name = Cici,tel_ = 080xxxx
id = 0001
name = Bill,tel_ = 09xxxx
id = g0001
name = gBill,tel_ = g09xxxx
id = 0002
name = Jerry,tel_ = 08xxxx
```

## 改變父類存取權限

使用using 子類可以改變父類成員變數的權限，但無法修改父類權限為private成員變數。

以下的例子public權限改為private，protected改為public
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  string name_;
 protected:
  string tel_;
 private:
  string id_;
};
class Child:public Parent {
 public:
  // 父類別protected成員變數改成public
  using Parent::tel_;
 private:
  // 父類別public成員變數改成private
  using Parent::name_;
};
int main() {
  Child child;
  child.tel_ = "087000";
  return 0;
}
{% endhighlight %}

## 父類與子類建構子解構子呼叫順序

建立子類物件，呼叫父類建構子->呼叫子類建構子

銷毀子類物件，呼叫子類解構子->呼叫父類解構子

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  Parent() {cout << "Parent建構子" << endl;}
  ~Parent() {cout << "Parent解構子" << endl;}
};
class Child:public Parent {
 public:
  Child() {cout << "Child建構子" << endl;}
  ~Child() {cout << "Child解構子" << endl;}
};
int main() {
  Child child;
  return 0;
}
{% endhighlight %}

```
Parent建構子
Child建構子
Child解構子
Parent解構子
```