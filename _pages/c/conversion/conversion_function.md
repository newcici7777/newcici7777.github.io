---
title: 建構子轉型
date: 2025-02-23
keywords: c++, conversion function, explicit conversion, implicit conversion
---

Prerequisites:
- [RVO][1]
- [建構子參數只有一個，可使用指派運算子][2]

conversion function又稱作轉型函式，主要用來將一個類別轉型成另一個類別的函式，以下例子有從char轉型成Student類別，有從double轉型成Student類別，有從int轉型成Student類別。

## explicit顯式轉型

語法
```
(類型)表達式
類型(表達式)
```

顯式轉型就是告訴編譯器要轉型的型別，例如下方程式碼，等號右邊明確定義Student型別。
{% highlight c++ linenos %}
Student student2 = Student("student2");
Student student3 = (Student)"student2";
{% endhighlight %}

## implicit自動轉型
等號右邊是const char\* 型別，由編譯器去尋找能接受const char\* 的建構子來建立物件。
{% highlight c++ linenos %}
Student student3 = "student3";
{% endhighlight %}

### 多個單一參數，但型別不同建構子

以下分別有const char\*建構子與double建構子
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string name;
  double weight;
  int age;
  Student(){};
  // 解構子
  ~Student() {}
  // 參數只有一個建構子
  Student(const char* name) {
    this->name = name;
  }
  // 參數是double
  Student(double weight) {
    this->weight = weight;
  }
};
int main() {
  // 呼叫建構子參數為const char* name
  Student student4 = "student4";
  // 呼叫建構子參數為double
  Student student5 = 58.5;
  return 0;
}
{% endhighlight %}


### 多個參數，但第2個參數以後皆有預設值
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string name;
  double weight;
  int age;
  Student(){};
  // 解構子
  ~Student() {}
  // 參數第2個以後都有預設參數
  Student(int age, double weight = 50) {
    this->age = age;
    this->weight = weight;
  }
};
int main() {
  // 呼叫建構子第1個參數為int，第2個參數是預設
  Student student6 = 18;
  return 0;
}
{% endhighlight %}

### 先建立物件，再自動轉型的方式建立物件
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string name;
  double weight;
  int age;
  Student(){};
  // 解構子
  ~Student() {}
  // 參數第2個以後都有預設參數
  Student(int age, double weight = 50) {
    this->age = age;
    this->weight = weight;
  }
};
int main() {
  // 呼叫沒有參數的建構子
  Student student4;
  // 等號右邊建立匿名物件(呼叫1個參數的建構子)，再把匿名物件指派給student4(等號左邊)
  student4 = "student4";
  return 0;
}
{% endhighlight %}

### 函式的參數是字串，再自動轉型的方式建立物件
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string name;
  double weight;
  int age;
  Student(){};
  // 解構子
  ~Student() {}
  // 參數只有一個建構子
  Student(const char* name) {
    this->name = name;
  }
  // 參數是double
  Student(double weight) {
    this->weight = weight;
  }
  // 參數第2個以後都有預設參數
  Student(int age, double weight = 50) {
    this->age = age;
    this->weight = weight;
  }
};
void printStudent(Student s) {
  cout << "s的名字:" << s.name << endl;
}
int main() {
  printStudent("Bill");
  return 0;
}
{% endhighlight %}
```
s的名字:Bill
```

### 函式傳回值為自動轉型

{% highlight c++ linenos %}
Student createStudent() {
  return "Alice";
}
int main() {
  Student student7 = createStudent();
  cout << "student7 name = " << student7.name << endl;
  return 0;
}
{% endhighlight %}
```
student7 name = Alice
```
### 若自動轉型的型態與參數型態不同，但可以透過自動轉型，建立物件
以下例子是由char轉成int，會呼叫int的建構子參數
{% highlight c++ linenos %}
class Student {
public:
  string name;
  double weight;
  int age;
  Student(){};
  // 解構子
  ~Student() {}
  Student(int age) {
    this->age = age;
    this->weight = weight;
  }
};
Student createStudent2() {
  char a = 97;
  return a;
}
int main() {
  Student student8 = createStudent2();
  cout << "student8 age = " << student8.age << endl;
  return 0;
}
{% endhighlight %}
```
student8 age = 97
```

### 建構子前加上explicit，就不能使用自動轉型函式
但可以使用顯式轉型
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string name;
  double weight;
  int age;
  Student(){};
  // 解構子
  ~Student() {}
  // 參數是double
  explicit Student(double weight) {
    this->weight = weight;
  }
};
int main() {
  // 顯式轉型
  Student student5 = Student(58.5);
  // 無法使用以下自動轉型
  //Student student5 = 58.5;
  return 0;
}
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string name;
  double weight;
  int age;
  Student(){};
  // 解構子
  ~Student() {}
  // 參數只有一個建構子
  Student(const char* name) {
    this->name = name;
  }
  // 參數是double
  explicit Student(double weight) {
    this->weight = weight;
  }
  // 參數第2個以後都有預設參數
  Student(int age, double weight = 50) {
    this->age = age;
    this->weight = weight;
  }
};
void printStudent(Student s) {
  cout << "s的名字:" << s.name << endl;
}
Student createStudent() {
  return "Alice";
}
Student createStudent2() {
  char a = 97;
  return a;
}
int main() {
  // 呼叫一個參數的建構子
  Student student1("student1");
  cout << "student1 name = " << student1.name << endl;
  // 強制轉型
  // 手動指定轉型過程（例如 Student student2 = Student("student2");，這就是顯式轉型
  Student student2 = Student("student2");
  cout << "student2 name = " << student2.name << endl;
  // 使用等於(=)指派運算子呼叫只有一個參數的建構子
  // 自動轉型："student3" 會被轉型為 Student("student3")。
  // "student3" 原本是 const char* 型別，不是 Student。
  //  C++編譯器發現 Student 類別有一個能接受 const char* 的建構子，所以它自動執行型別轉型，並呼叫這個建構子來建立 Student 物件
  // 這種自動進行的轉型稱為自動轉型
  Student student3 = "student3";
  cout << "student3 name = " << student3.name << endl;
  // 建立student4物件
  // 呼叫沒有參數的建構子
  Student student4;
  // 等號右邊建立匿名物件(呼叫1個參數的建構子)，再把匿名物件指派給student4(等號左邊)
  student4 = "student4";
  cout << "student4 name = " << student4.name << endl;
  // 呼叫建構子參數為double
  Student student5 = Student(58.5);
  cout << "student5 weight = " << student5.weight << endl;
  // 呼叫建構子第1個參數為int，第2個參數是預設
  Student student6 = 18;
  cout << "student6 age = " << student6.age << ",student6 weight = " << student6.weight << endl;
  printStudent("Bill");
  Student student7 = createStudent();
  cout << "student7 name = " << student7.name << endl;
  Student student8 = createStudent2();
  cout << "student8 age = " << student8.age << endl;
  Student student9 = (Student)"student9";
  cout << "student9 name = " << student9.name << endl;
  return 0;
}
{% endhighlight %}
```
student1 name = student1
student2 name = student2
student3 name = student3
student4 name = student4
student5 weight = 58.5
student6 age = 18,student6 weight = 50
s的名字:Bill
student7 name = Alice
student8 age = 97
student9 name = student9
```


[1]: {% link _pages/c/editor/rvo.md %}
[2]: {% link _pages/c/class/constructor.md %}#建構子參數只有一個，可使用指派運算子