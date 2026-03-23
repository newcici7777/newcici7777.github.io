---
title: 成員變數
date: 2024-10-14
keywords: c++, Member Variables 
---
## 成員變數權限
類別成員變數權限預設是private，結構成員變數權限預設是public。<br>

public與private與其它權限，可以在類別中出現很多次，以下public就出現2次，private出現2次。<br>

{% highlight c++ linenos %}
class Student {
public:
  char name[50];
private:
  char address[100];
public:
  int age;
private:
  char father[50];
};
{% endhighlight %}

## 成員變數命名
命名方式使用`成員變數_`，成員變數名 \+ 加上底線_。<br>
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s;
  s.name_ = "Bill";
  return 0;
}
{% endhighlight %}

## 成員變數未初始化
在[變數預設值][4]與[函式區域變數初始值][3]文章中提過，區域變數不會有預設值，所以印出區域變數都是垃圾資料。<br>
{% highlight c++ linenos %}
int main() {
  int n;
  cout << n << endl;
  return 0;
}
{% endhighlight %}
```
15824
```

相同的，物件的成員變數也不會有初始值，所以印出來都是垃圾資料。<br>
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s;
  cout << s.name_ << endl;
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
H\211\330H\203\304[]\303UH\211\345H\205\366tTE1\311A\2700
-1074793600
```

### 成員變數初始化
指標變數，可以設nullptr;
{% highlight c++ linenos %}
class Student {
 public:
  // 成員變數初始化
  const char* name_ = nullptr;
  int id_ = 0;
  Student() {}
};
int main() {
  Student s;
  if (s.name_ == nullptr) {
      cout << "name_ is nullptr" << endl;
  } else {
      cout << "name_: " << s.name_ << endl;
  }
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
name_ is nullptr
0
```

## 成員函式在類別外定
- [函式宣告與定義][2]

print()成員函式程式碼在類別之外定義，定義方式如下。

```
傳回值 類別名::函式名(){程式碼}
```

{% highlight c++ linenos %}
class Student {
public:
  char name_[50];
  //宣告函式
  void print();
};
//類別外部定義
void Student::print() {
  cout << "test" << endl;
}
int main() {
  Student student;
  student.print();
  return 0;
}
{% endhighlight %}
```
test
```

## 成員變數是類別
在Java的世界中，只有new的那一刻才會呼叫類別的建構子。

但在C++的世界中，執行到Family m_family;就會呼叫類別的建構子，並且在記憶體建立此物件的位址。

{% highlight c++ linenos %}
class Student {
public:
  Family m_family;
};
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Family {
public:
  string mon;
  string dad;
  Family(){
    cout << "Family 建構子" << endl;
  }
};
class Student {
public:
  Family m_family;
};
int main() {
  Student student;
  return 0;
}
{% endhighlight %}
```
Family 建構子
```

由執行結果可以知道，建立student物件的時候，就會建立Family物件。




[1]: {% link _pages/c/function/func_inline.md %}
[2]: {% link _pages/c/function/func_def.md %}
[3]: {% link _pages/c/function/callByValue.md %}#區域變數初始值
[4]: {% link _pages/c/basic/scope.md %}#預設值
