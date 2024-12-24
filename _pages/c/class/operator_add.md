---
title: operator+()
date: 2024-10-28
keywords: c++, operator+ 
---

## addScore()函式

以下程式碼，在類別外定義addScore()函式，並在Student類別中把addScore()設為friend(友情/朋友)，代表addScore()類別外的函式可以使用Student的private成員變數。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
  //私有成員變數
  int m_math;
  int m_english;
  int m_chinese;
public:
  //將類別外的函式設為friend，才能讀取物件私有成員變數
  friend void addScore(Student& s, int score);
  string m_name;
  Student(){}
  //建構子，姓名，數學成績，英文成績，國文成績
  Student(string name, int math, int english, int chinese) {
    m_name = name;
    m_math = math;
    m_english = english;
    m_chinese = chinese;
  }
  //印出學生姓名與分數
  void print() {
    cout << "name = " << m_name << endl;
    cout << "math = " << m_math << endl;
    cout << "english = " << m_english << endl;
    cout << "chinese = " << m_chinese << endl;
  }
};
//為每個科目增加分數
void addScore(Student& s, int score) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
}
int main() {
  //建立物件
  Student student("Bill", 50, 60, 70);
  //為每個分數增加15分
  addScore(student, 15);
  //印出學生姓名與各科分數
  student.print();
  return 0;
}
{% endhighlight %}
```
name = Bill
math = 65
english = 75
chinese = 85
```

## operator+()函式

把addScore函式名全改成operator+

{% highlight c++ linenos %}
class Student {
  //私有成員變數
  int m_math;
  int m_english;
  int m_chinese;
public:
  //將類別外的函式設為friend，才能讀取物件私有成員變數
  friend void operator+(Student& s, int score);
  string m_name;
  Student(){}
  //建構子，姓名，數學成績，英文成績，國文成績
  Student(string name, int math, int english, int chinese) {
    m_name = name;
    m_math = math;
    m_english = english;
    m_chinese = chinese;
  }
  //印出學生姓名與分數
  void print() {
    cout << "name = " << m_name << endl;
    cout << "math = " << m_math << endl;
    cout << "english = " << m_english << endl;
    cout << "chinese = " << m_chinese << endl;
  }
};
//為每個科目增加分數
void operator+(Student& s, int score) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
}
int main() {
  //建立物件
  Student student("Bill", 50, 60, 70);
  //為每個分數增加15分
  operator+(student, 15);
  //印出學生姓名與各科分數
  student.print();
  return 0;
}
{% endhighlight %}

## student + 15

如果函式名是operator+(student,30)，寫法就可以換成student + 30，student是operator+()第一個參數，30是operator+()第二個參數。

{% highlight c++ linenos %}
class Student {
  //私有成員變數
  int m_math;
  int m_english;
  int m_chinese;
public:
  //將類別外的函式設為friend，才能讀取物件私有成員變數
  friend void operator+(Student& s, int score);
  string m_name;
  Student(){}
  //建構子，姓名，數學成績，英文成績，國文成績
  Student(string name, int math, int english, int chinese) {
    m_name = name;
    m_math = math;
    m_english = english;
    m_chinese = chinese;
  }
  //印出學生姓名與分數
  void print() {
    cout << "name = " << m_name << endl;
    cout << "math = " << m_math << endl;
    cout << "english = " << m_english << endl;
    cout << "chinese = " << m_chinese << endl;    
  }
};
//為每個科目增加分數
void operator+(Student& s, int score) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
}
int main() {
  //建立物件
  Student student("Bill", 50, 60, 70);
  //為每個分數增加15分
  student + 15;
  //印出學生姓名與各科分數
  student.print();
  return 0;
}
{% endhighlight %}
```
name = Bill
math = 65
english = 75
chinese = 85
```

## 回傳值型態是物件

```
student = student + 15;
```
以上的程式碼，是要把運算的結果回傳給型態是Student的物件，所以必須把回傳值型態改成物件的參考。

{% highlight c++ linenos %}
Student& operator+(Student& s, int score) 
{% endhighlight %}

運算的過程也可以是很多數字的相加，不變的是，要把運算的結果回傳給型態是Student的物件。

```
student = student + 15 + 5 + 5;
```
以上的運算過程，函式的呼叫如下

```
student = (((student + 15) + 5) + 5);
```
每一個括號就有一個回傳值，+加號左邊的參數是operator+()第一個參數，+加號右邊是operator+()第二個參數。

再次分解如下

```
student = operator+(operator+(operator+(student, 15), 5), 5);
```

程式的回傳值型態改寫如下

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
  //私有成員變數
  int m_math;
  int m_english;
  int m_chinese;
public:
  //將類別外的函式設為friend，才能讀取物件私有成員變數
  friend Student& operator+(Student& s, int score);
  string m_name;
  Student(){}
  //建構子，姓名，數學成績，英文成績，國文成績
  Student(string name, int math, int english, int chinese) {
    m_name = name;
    m_math = math;
    m_english = english;
    m_chinese = chinese;
  }
  //印出學生姓名與分數
  void print() {
    cout << "name = " << m_name << endl;
    cout << "math = " << m_math << endl;
    cout << "english = " << m_english << endl;
    cout << "chinese = " << m_chinese << endl;   
  }
};
//為每個科目增加分數
Student& operator+(Student& s, int score) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
  return s;
}
int main() {
  //建立物件
  Student student("Bill", 50, 60, 70);
  //為每個分數增加15分
  student = student + 15;
  //印出學生姓名與各科分數
  student.print();
  return 0;
}
{% endhighlight %}

## operator+()成員函式

之前的方式是把類別之外的函式透過friend實現，接下來將operator+()在類別內實作。

把operator+()變為成員函式，成員函式本身就有隱藏的this指標，指向呼叫函式的物件，所以原本的第一個參數可以刪掉。

```
Student& operator+(int score){}
```
而原本

{% highlight c++ linenos %}
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
{% endhighlight %}
可以改為
{% highlight c++ linenos %}
  m_math += score;
  m_chinese += score;
  m_english += score;
{% endhighlight %}

原本的friend函式也可以刪掉
{% highlight c++ linenos %}
friend Student& operator+(Student& s, int score);
{% endhighlight %}

回傳值也改為隱藏的this指標，this指標為物件存放的記憶體位址，使用取值運算子，回傳物件。

```
return *this;
```

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
  //私有成員變數
  int m_math;
  int m_english;
  int m_chinese;
public:
  string m_name;
  Student(){}
  //建構子，姓名，數學成績，英文成績，國文成績
  Student(string name, int math, int english, int chinese) {
    m_name = name;
    m_math = math;
    m_english = english;
    m_chinese = chinese;
  }
  Student& operator+(int score) {
    m_math += score;
    m_chinese += score;
    m_english += score;
    return *this;
  }
  //印出學生姓名與分數
  void print() {
    cout << "name = " << m_name << endl;
    cout << "math = " << m_math << endl;
    cout << "english = " << m_english << endl;
    cout << "chinese = " << m_chinese << endl;
  }
};
int main() {
  //建立物件
  Student student("Bill", 50, 60, 70);
  //為每個分數增加15分
  student = student + 15 + 5;
  //印出學生姓名與各科分數
  student.print();
  return 0;
}
{% endhighlight %}

## operator+()參數替換

若想使用以下的方式，數值在加號+左邊，物件在加號+右邊。

```
student = 15 + student; 
```
只能使用類別外的friend函式實現，無法使用成員函式實現。

增加函式如下

{% highlight c++ linenos %}
Student& operator+(int score, Student& s){...}
{% endhighlight %}

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
  //私有成員變數
  int m_math;
  int m_english;
  int m_chinese;
public:
  //將類別外的函式設為friend，才能讀取物件私有成員變數
  friend Student& operator+(Student& s, int score);
  friend Student& operator+(int score, Student& s);
  string m_name;
  Student(){}
  //建構子，姓名，數學成績，英文成績，國文成績
  Student(string name, int math, int english, int chinese) {
    m_name = name;
    m_math = math;
    m_english = english;
    m_chinese = chinese;
  }
  //印出學生姓名與分數
  void print() {
    cout << "name = " << m_name << endl;
    cout << "math = " << m_math << endl;
    cout << "english = " << m_english << endl;
    cout << "chinese = " << m_chinese << endl;
  }
};
//為每個科目增加分數
Student& operator+(Student& s, int score) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
  return s;
}
Student& operator+(int score, Student& s) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
  return s;
}
int main() {
  //建立物件
  Student student("Bill", 50, 60, 70);
  //為每個分數增加15分
  student = 15 + student;
  //印出學生姓名與各科分數
  student.print();
  return 0;
}
{% endhighlight %}

## operator+()參數都為物件

若想使用以下的方式。

{% highlight c++ linenos %}
  Student s1("Bill", 50, 60, 70);
  Student s2("Tom", 50, 60, 70);
  s1 = s1 + s2;
{% endhighlight %}

寫一個operator+傳入的參數為2個物件

{% highlight c++ linenos %}
Student& operator+(Student& s1, Student& s2){}
{% endhighlight %}

程式碼如下
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
  //私有成員變數
  int m_math;
  int m_english;
  int m_chinese;
public:
  //將類別外的函式設為friend，才能讀取物件私有成員變數
  friend Student& operator+(Student& s, int score);
  friend Student& operator+(int score, Student& s);
  friend Student& operator+(Student& s1, Student& s2);
  string m_name;
  Student(){}
  //建構子，姓名，數學成績，英文成績，國文成績
  Student(string name, int math, int english, int chinese) {
    m_name = name;
    m_math = math;
    m_english = english;
    m_chinese = chinese;
  }
  //印出學生姓名與分數
  void print() {
    cout << "name = " << m_name << endl;
    cout << "math = " << m_math << endl;
    cout << "english = " << m_english << endl;
    cout << "chinese = " << m_chinese << endl;
  }
};
//為每個科目增加分數
Student& operator+(Student& s, int score) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
  return s;
}
Student& operator+(int score, Student& s) {
  s.m_math += score;
  s.m_chinese += score;
  s.m_english += score;
  return s;
}
Student& operator+(Student& s1, Student& s2) {
  s1.m_math += s2.m_math;
  s1.m_chinese += s2.m_chinese;
  s1.m_english += s2.m_english;
  return s1;
}
int main() {
  //建立物件
  Student s1("Bill", 50, 60, 70);
  Student s2("Tom", 50, 60, 70);
  s1 = s1 + s2;
  //印出學生姓名與各科分數
  s1.print();
  return 0;
}
{% endhighlight %}

## 混合使用

以上程式碼支援以下寫法

```
s1 = s1 + 10 + 5 + s2;
```

{% highlight c++ linenos %}
int main() {
  //建立物件
  Student s1("Bill", 50, 60, 70);
  Student s2("Tom", 50, 60, 70);
  s1 = s1 + 10 + 5 + s2;
  //印出學生姓名與各科分數
  s1.print();
  return 0;
}
{% endhighlight %}