---
title: 建立物件
date: 2026-03-23
keywords: C++, Create Object
---
## 建立物件的方式
常用的是方式一跟方式二，其它都可以略過。

方式一
```
類別 物件;
Student s;
```

方式二
```
類別 物件 = new 類別();
Student s = new Student();
```

方式三<br>
注意！以下沒有new
```
類別 物件 = 類別();
Student s = Student();
```

方式四
```
類別 物件變數{}
Student s{}
```

以下是錯誤！！！
```
Student student();
```

## 建立在Stack中
![img]({{site.imgurl}}/c++/create_obj_stack.png)<br>

程式碼
{% highlight python linenos %}
#include <iostream>
#include "main.h"
using namespace std;
class Student {
 public:
  const char* name_;
};
int main() {
  Student s;
  return 0;
}
{% endhighlight %}

## 建立在Heap中
![img]({{site.imgurl}}/c++/create_obj_heap.png)<br>

程式碼
{% highlight c++ linenos %}
#include <iostream>
#include "main.h"
using namespace std;
class Student {
 public:
  const char* name_;
};
int main() {
  Student* s = new Student();
  return 0;
}
{% endhighlight %}

-------------------------------------------
以下為舊文章

其它建立物件的方式

基本型態與指標預設值如下:

|型態|預設值|
|int|0|
|char|`\0`|
|float|0.0|
|double|0.0|
|指標類型|NULL|

使用以下方法，成員變數就會設為預設值。

### 類別()
類別()會把成員變數初始化為預設值，注意！<span class="markline">不能有自訂建構子()</span>，有自訂建構子()，印出來仍是垃圾資料。<br>
```
類別 物件變數 = 類別()
Student s = Student()
```

無建構子程式碼。
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s = Student();
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

有建構子程式碼:
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
  Student() {}
};
int main() {
  Student s = Student();
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
name_: H\211\330H\203\304[]\303UH\211\345H\205\366tTE1\311A\2700
-1074793600
```

### 物件變數{}
物件變數{}會把成員變數初始化為預設值，注意！<span class="markline">不能有自訂建構子()</span>，有自訂建構子()，印出來仍是垃圾資料。<br>
```
類別 物件變數{}
Student s{}
```

無建構子程式碼。
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s{};
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

有建構子程式碼:
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
  Student() {}
};
int main() {
  Student s{};
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
name_: H\211\330H\203\304[]\303UH\211\345H\205\366tTE1\311A\2700
-1074793600
```
