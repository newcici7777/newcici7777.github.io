---
title: 指標指向結構
date: 2024-07-03
keywords: c++, struct
---

## 宣告指標

```
結構 指標變數 = 結構變數地址;
Student *ptr = &student;
```

## 存取結構成員

### 使用取值運算子

```
(*指標變數).成員 
```

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
typedef struct{
  //學生姓名
  char* name;
  //學號
  int id;
}Student;
int main() {
  Student student = {"marry", 1};
  Student *ptr = &student;
  cout << "姓名 : " << (*ptr).name << endl;
  cout << "學號 : " << (*ptr).id << endl;
  return 0;
}
{% endhighlight %}

### 使用->

```
指標變數->成員 
```
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
typedef struct{
  //學生姓名
  char* name;
  //學號
  int id;
}Student;
int main() {
  Student student = {"marry", 1};
  Student *ptr = &student;
  cout << "姓名 : " << ptr->name << endl;
  cout << "學號 : " << ptr->id << endl;
  return 0;
}
{% endhighlight %}
