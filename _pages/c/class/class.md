---
title: 類別與物件
date: 2024-10-14
keywords: c++, class 
---

## 權限

類別權限預設是private，結構權限預設是public

public與private與其它權限，可以在類別中出現很多次，以下public就出現2次，private出現2次

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

## 成員屬性命名

命名方式使用`m_成員屬性`名字或`成員屬性_`

- 前綴加上m_
- 後綴加上底線_

{% highlight c++ linenos %}
class Student {
public:
  string m_name;
public:
  Student() {
  }
  void print() {
    cout << "name: " << m_name << endl;
  }
};
int main() {
  Student s;
  s.m_name = "Bill";
  s.print();
  return 0;
}
{% endhighlight %}
```
name: Bill
```

## 類別作為函式參數

使用類別作為函式參數，都是使用參考&的方式傳遞。

{% highlight c++ linenos %}
class Student {
public:
  string m_name;
public:
  Student() {
  }
  void print() {
    cout << "name: " << m_name << endl;
  }
};
void func1(const Student& s) {
  cout << s.m_name << endl;
}
int main() {
  Student s;
  s.m_name = "Bill";
  func1(s);
  return 0;
}
{% endhighlight %}

```
Bill
```

## 類別中的函式自動變為內嵌函式(inline)

- [inline][1]

類別中的函式自動變為內嵌函式，但不是在類別中的函式不會變成內嵌函式。

以下print()是內嵌函式

{% highlight c++ linenos %}
class Student {
public:
  char m_name[50];
  //函式自動變為內嵌函式
  void print() {
  }
};
{% endhighlight %}


## 成員函式在類別外定義

- [函式宣告與定義][2]

print()成員函式程式碼在類別之外定義，定義方式如下。

```
回傳值 類別名::函式名(){程式碼}
```

{% highlight c++ linenos %}
class Student {
public:
  char m_name[50];
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

[1]: {% link _pages/c/function/func_inline.md %}
[2]: {% link _pages/c/function/func_def.md %}