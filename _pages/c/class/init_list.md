---
title: 建構子初始化列表
date: 2024-10-18
keywords: c++, Initialization list of constructors)
---

## 語法

建構子初始化列表(Initialization list of constructors)，因為不知道繁體中文的名稱，先以大陸的名稱來替代。

- 成員變數已經在初始化列表，不應該再建構子中再設值

語法如下

```
類別名(資料型態 參數名1, 資料型態 參數名2, ...):成員變數1(參數名1),成員變數2(參數名2), ... {}
```

下方有參數的建構子，使用初始化列表，初使化成員變數。

## 程式碼
{% highlight c++ linenos %}
class Student {
public:
  string m_name;
  int m_age;
public:
  Student() {
    cout << "沒參數建構子" << endl;
  }
  Student(string name, int age):m_name(name),m_age(age) {
    cout << "初始化列表建構子" << endl;
  }
  ~Student() {
    cout << "解構子" << endl;
  }
  void print() {
    cout << "name: " << m_name << endl;
    cout << "age: " << m_age << endl;
  }
};
int main() {
  Student s1("cici", 18);
  s1.print();
  return 0;
}
{% endhighlight %}
```
初始化列表建構子
name: cici
age: 18
解構子
```

## 初始化列表與運算式

下方有參數的建構子，使用初始化列表並加上運算式，初使化成員變數。

{% highlight c++ linenos %}
class Student {
public:
  string m_name;
  int m_age;
public:
  Student() {
    cout << "沒參數建構子" << endl;
  }
  Student(string name, int age):m_name("漂亮的" + name),m_age(age + 10) {
    cout << "初始化列表建構子" << endl;
  }
  ~Student() {
    cout << "解構子" << endl;
  }
  void print() {
    cout << "name: " << m_name << endl;
    cout << "age: " << m_age << endl;
  }
};
int main() {
  Student s1("cici", 18);
  s1.print();
  return 0;
}
{% endhighlight %}
```
初始化列表建構子
name: 漂亮的cici
age: 28
解構子
```
