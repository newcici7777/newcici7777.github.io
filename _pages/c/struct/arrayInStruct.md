---
title: 結構中的陣列
date: 2024-07-11
keywords: c++, Array within a Structure
---

## 結構中定義陣列

定義成員score[3]

{% highlight c++ linenos %}
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
  int score[3];
};
{% endhighlight %}

## 初始化結構中的陣列

### 方式一

{% highlight c++ linenos %}
  Student student = {"Mary", 1, {60,70,80}};
{% endhighlight %}

### 方式二

{% highlight c++ linenos %}
  Student student = {"Mary", 1, 60,70,80};
{% endhighlight %}

## 存取結構中的陣列

修改的寫法

```
結構.成員[索引] = 值
```
{% highlight c++ linenos %}
  student.score[0] = 50;
  student.score[1] = 60;
  student.score[2] = 70;
{% endhighlight %}

讀取寫法
```
cout << 結構.成員[索引] << endl;
```
{% highlight c++ linenos %}
  for(int i = 0; i < 3; i++) {
    cout << "score[" << i << "] = " <<student.score[i] << endl;
  }
{% endhighlight %}


## 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;

struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
  //結構中的陣列
  int score[3];
};
int main() {
  //初始化
  Student student = {"Mary", 1, 60,70,80};
  //修改結構中的陣列
  student.score[0] = 50;
  student.score[1] = 60;
  student.score[2] = 70;
  //印出陣列元素
  for(int i = 0; i < 3; i++) {
    cout << "score[" << i << "] = " <<student.score[i] << endl;
  }
  return 0;
}

{% endhighlight %}

## 傳遞結構中的陣列給函式

不管結構中的陣列是一維或多維，傳遞結構中的陣列給函式，函式參數都是指向結構的指標。

```
傳回值 函式名(結構指標* 指標變數名);
void printParent(Student* student);
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
const int PARENT_SIZE = 2;
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
  //定義二維陣列
  char parent[PARENT_SIZE][100];
};
//函式參數是指向結構的指標
void printParent(Student* student) {
  for(int i = 0; i < PARENT_SIZE; i++) {
    //因為是指標，所以存取成員使用->
    cout << student->parent[i] << endl;
  }
}
int main() {
  //初始化
  Student student = {"Mary", 1, {"Alice","Bill"}};
  //把結構的位址傳入函式
  printParent(&student);
  return 0;
}
{% endhighlight %}

```
Alice
Bill
```
