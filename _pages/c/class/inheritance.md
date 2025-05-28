---
title: 繼承
date: 2025-02-24
keywords: c++, inheritance
---

## 子類別繼承父類別
子類別繼承父類別，子類別可以使用父類別的成員變數與成員函式。
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
  // 使用父類別的setName成員函式
  child.setName("Cici");
  // 使用父類別的setTel成員函式
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

若繼承方式是protected，父類別的public成員函式與成員變數會降級成protected，影嚮到孫子類別存取祖父類別的成員函式與變數。

若繼承方式是private，父類別的public成員函式與成員變數會降級成private，影嚮到孫子類別無法存取祖父類別的成員函式與變數。

|繼承方式	|在 main()存取父類別public成員函式|在子類別存取父類別public成員函式|在孫子類別存取祖父類別public成員函式|
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
    // 父類別id_成員變數是private無法使用
    cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
  }
};
//繼承方式為protected，父類別的public成員變數與函式全變成protected
class Child2:protected Parent {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 父類別id_成員變數是private無法使用
   // 父類別的成員函式降級protected，但仍能在子類別成員函式使用
   // 無法在main()外部使用父類public的setName(),setTel(),printId()
   setName(name);
   setTel(tel);
   setId(id);
   printId();
   // 父類別的成員變數的仍可給child成員函式使用
   cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
 }
};
class GrandChild2:protected Child2 {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 祖父id_成員變數是private無法使用
   // 祖父的成員函式setName(),setTel(),printId()降級protected，但仍能在孫子類別成員函式使用
   // 無法在main()外部使用祖父類public的setName(),setTel(),printId()
   setName(name);
   setTel(tel);
   setId(id);
   printId();
   // 祖父類別的成員變數的仍可給child成員函式使用
   cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
 }
};
class Child3:private Parent {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 父類別id_成員變數是private無法使用
   // 父類別的成員函式setName(),setTel(),printId()降級private, 但仍能在子類別成員函式使用，孫子類別無法使用
   // 無法在main()外部使用父類public的setName(),setTel(),printId()
   setName(name);
   setTel(tel);
   setId(id);
   printId();
   // 父類別的成員變數的仍可給child成員函式使用
   cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
 }
};
class GrandChild3:private Child3 {
 public:
 void print(const string& name, const string& tel, const string& id) {
   // 祖父類別id_成員變數是private無法使用
   // 祖父類別的成員函式setName(),setTel(),setId(),printId()降級private無法使用
   // 祖父類別的public方法setName(),setTel(),setId(),printId()全部降級成private無法使用
   //setName(name);
   //setTel(tel);
   //setId(id);
   //printId();
   // 祖父類別的成員變數降級private無法使用
   // cout << "name = " << name_ << ",tel_ = " << tel_ << endl;
 }
};
int main() {
  // 建立child1物件
  // 因為繼承方式是public，所以父類別的public成員函式可以使用
  Child1 child1;
  child1.setName("Cici");
  child1.setTel("080xxxx");
  child1.print();
  Child2 child2;
  // 因為繼承方式是protected，所以父類別的public成員函式全降級變為protected成員函式
  // protected就不能在main()外部使用父類別的public成員函式setName()與setTel()
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

## 改變父類別存取權限

使用using 子類別可以改變父類別成員變數的權限，但無法修改父類別權限為private成員變數。

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
  // 父類別別protected成員變數改成public
  using Parent::tel_;
 private:
  // 父類別別public成員變數改成private
  using Parent::name_;
};
int main() {
  Child child;
  child.tel_ = "087000";
  return 0;
}
{% endhighlight %}

## 繼承的建構子與解構子

- 建立子類別物件，呼叫父類別建構子->呼叫子類別建構子

- 銷毀子類別物件，呼叫子類別解構子->呼叫父類別解構子

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

## 繼承的範圍運算子
以下的範例，父類別與子類別與孫類別都有print()成員函式，也都有name的成員變數，使用範圍運算子，可以明確指示出要使用祖父類別或父類別的成員函式或成員變數。

呼叫父類別成員變數語法
```
子類物件.父類別::成員變數
```

呼叫父類別成員函式語法
```
子類物件.父類別::成員函式()
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  string name_ = "Parent";
  void print() {
    cout << "Parent name:" << name_ << endl;
  }
};
class Child:public Parent {
 public:
  string name_ = "Child";
  void print() {
    cout << "Child name:" << name_ << endl;
  }
};
class GrandChild:public Child {
 public:
  string name_ = "GrandChild";
 void print() {
   cout << "GrandChild name:" << name_ << endl;
 }
};
int main() {
  GrandChild grandChild;
  cout << "Parent name = " << grandChild.Parent::name_ << endl;
  cout << "Child name = " << grandChild.Child::name_ << endl;
  cout << "GrandChild name = " << grandChild.GrandChild::name_ << endl;
  cout << "------------------------" << endl;
  grandChild.Parent::print();
  grandChild.Child::print();
  grandChild.GrandChild::print();
  return 0;
}
{% endhighlight %}
```
Parent name = Parent
Child name = Child
GrandChild name = GrandChild
------------------------
Parent name:Parent
Child name:Child
GrandChild name:GrandChild
```

也可以使用一層層呼叫方式，呼叫成員變數。呼叫方式子類別::父類別::父類別的成員變數

{% highlight c++ linenos %}
cout << "Parent name = " << grandChild.Child::Parent::name_ << endl;
{% endhighlight %}

## 父類別子類別大小

### sizeof
Prerequisites:

- [operator new delete][1]

以下的程式範例可以看出父類別有3個int變數(m1,m2,m3)，int大小為4byte，所以父類別總共為12byte<br>
子類別只有1個int變數(m4)，4byte，但子類別會繼承父類別的變數，所以12byte + 4byte = 16byte，因此子類別建立的記憶體大小為16byte。<br>
透過自建operator new()函式，可以看到建立Child的大小為16byte。<br>

使用以下語法可以看出父類別的大小
```
sizeof(父類別)
```
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void* operator new(size_t size) {
  cout << "全域new size = " << size << endl;
  void* ptr = malloc(size);
  cout << "全域 address = " << ptr << endl;
  return ptr;
}
void operator delete(void* ptr) {
  if (ptr == nullptr) return;
  cout << "全域 delete address = " << ptr << endl;
  free(ptr);
}
class Parent {
 public:
  int m1_ = 10;
  int m2_ = 20;
  int m3_ = 30;
};
class Child:public Parent {
 public:
  int m4_ = 40;
};
int main() {
  cout << "size of Parent = " << sizeof(Parent) << endl;
  cout << "size of Child = " << sizeof(Child) << endl;
  Child* ptr = new Child;
  return 0;
}
{% endhighlight %}
```
size of Parent = 12
size of Child = 16
全域new size = 16
全域 address = 0x600000008030
```

### 記憶體布局 Memory layout 

windows使用以下語法  
```
cl 原始檔案 /d1 reportSingleClassLayout類別名
cl test.cpp /d1 reportSingleClassLayoutTestA
```

mac使用以下語法  
-A 10 代表只印出10筆  
```
clang++ -Xclang -fdump-record-layouts -c 原始檔案.cpp | grep -A 10 "class 類別名"
clang++ -Xclang -fdump-record-layouts -c test1.cpp | grep -A 10 "class Parent"
```

#### Parent
拿上一個範例的程式碼，顯示出來的記憶體布局如下:  
```
         0 | class Parent
         0 |   int m1_
         4 |   int m2_
         8 |   int m3_
           | [sizeof=12, dsize=12, align=4,
           |  nvsize=12, nvalign=4]
```

- m1_從0byte開始
- m2_從4byte開始
- m3_從8byte開始

int整數大小是4byte，所以Parent類別的大小為12

#### Child

```
clang++ -Xclang -fdump-record-layouts -c test1.cpp | grep -A 10 "class Child"
```

```
         0 | class Child
         0 |   class Parent (base)
         0 |     int m1_
         4 |     int m2_
         8 |     int m3_
        12 |   int m4_
           | [sizeof=16, dsize=16, align=4,
           |  nvsize=16, nvalign=4]
```

- Parent m1_從0byte開始
- Parent m2_從4byte開始
- Parent m3_從8byte開始
- Child m4_從12byte開始

子類別本身包含了父類別的成員變數，所以Child的大小為16byte。

[1]: {% link _pages/c/dynamicMemory/operator_new_delete.md %}