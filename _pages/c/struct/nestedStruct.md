---
title: 巢狀結構
date: 2024-07-11
keywords: c++, nestedStruct
---

Prerequisites:

- [無法使用大括號修改單獨成員是字串陣列][1]



巢狀結構指的是結構中還有結構。


## 定義巢狀結構

結構中的結構要先定義。

{% highlight c++ linenos %}
struct Address{
  int zip;
  char addr[100];
};
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
  Address address;
};
{% endhighlight %}

## 初始化巢狀結構

初始化方式有2種。

### 方式一

巢狀結構的值放進大括號中。

```
結構 變數名 = {值1,值2,巢狀值1,巢狀值2};
```

{% highlight c++ linenos %}
  Student student = {"Mary", 1,338, "桃園市xxx"};
{% endhighlight %}

### 方式二

大括號中有大括號包著巢狀結構。

```
結構 變數名 = {值1,值2,{巢狀值1,巢狀值2}};
```

{% highlight c++ linenos %}
  Student student = {"Mary", 1,{338, "桃園市xxx"}};
{% endhighlight %}

## 修改巢狀結構

方式有二種。

### 大括號

```
變數名 = {值1,值2,巢狀值1,巢狀值2};
```

{% highlight c++ linenos %}
student = {"Bill", 2, 100, "台北市xxx"};
{% endhighlight %}

### 點運算子

```
結構變數.巢狀結構成員 = {巢狀值1,巢狀值2};
```

{% highlight c++ linenos %}
  student.name = "Jeff";
  student.id = 3;
  student.address = {300,"新竹縣xxxx"};
{% endhighlight %}

### 單獨修改巢狀結構中的成員

若巢狀結構中的成員是字串，修改方式如下

{% highlight c++ linenos %}
  strcpy(student.address.addr, "新竹縣xxxx");
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Address{
  int zip;
  char addr[100];
};
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
  Address address;
};
int main() {
  Student student = {"Mary", 1, {338, "桃園市xxx"}};
  student.name = "Jeff";
  student.id = 3;
  student.address.zip = 300;
  strcpy(student.address.addr, "新竹縣xxxx");
  cout << "After:" << endl;
  cout << student.name << endl;
  cout << student.id << endl;
  cout << student.address.zip << endl;
  cout << student.address.addr << endl;
  return 0;
}
{% endhighlight %}

```
After:
Jeff
3
300
新竹縣xxxx
```
### 讀取巢狀結構成員

```
結構變數.巢狀結構成員.成員
```

```
student.address.zip
```

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Address{
  int zip;
  char addr[100];
};
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
  Address address;
};
int main() {
  Student student = {"Mary", 1, {338, "桃園市xxx"}};
  cout << "Before:" << endl;
  student = {"Bill", 2, 100, "台北市xxx"};
  cout << student.name << endl;
  cout << student.id << endl;
  cout << student.address.zip << endl;
  cout << student.address.addr << endl;
  student.name = "Jeff";
  student.id = 3;
  student.address = {300,"新竹縣xxxx"};
  cout << "After:" << endl;
  cout << student.name << endl;
  cout << student.id << endl;
  cout << student.address.zip << endl;
  cout << student.address.addr << endl;
  return 0;
}
{% endhighlight %}

```
Before:
Bill
2
100
台北市xxx
After:
Jeff
3
300
新竹縣xxxx
```

[1]: {% link _pages/c/struct/struct_char.md %}#無法使用大括號修改單獨成員是字串陣列
