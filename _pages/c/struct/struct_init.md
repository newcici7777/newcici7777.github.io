---
title: 初始化結構
date: 2024-07-04
keywords: c++, struct
---
## 使用大括號{}初始化
```
結構  變數名 = {值1, 值2 ...}
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
  Student student = {"Mary", 1};
  cout << student.name << endl;
  cout << student.id << endl;
  return 0;
}
{% endhighlight %}
```
Mary
1
```

## 定義結構時初始化
```
struct 結構名{
	變數宣告
}變數1={值1, 值2 ...}, 變數2={值1, 值2 ...};
```
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
}s1 = {"Bill", 2}, s2 = {"Mary", 3};
int main() {
  cout << s1.name << endl;
  cout << s1.id << endl;
  cout << s2.name << endl;
  cout << s2.id << endl;
  return 0;
}
{% endhighlight %}
```
Bill
2
Mary
3
```

## 結構成員記憶體位址的值全初始化成00000000
### 方式1
{0}把成員初始化00000000。
```
struct 結構名{
	變數宣告
}結構變數={0};
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
}student = {0};
int main() {
  return 0;
}
{% endhighlight %}

```
結構  變數名 = {0}
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
  Student student = {0};
  return 0;
}
{% endhighlight %}

### 方式2
只寫大括號也是把成員初始化00000000。
```
struct 結構名{
	變數宣告
}結構變數={};
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
}student = {};
int main() {
  return 0;
}
{% endhighlight %}

```
結構  變數名 = {}
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
  Student student = {};
  return 0;
}
{% endhighlight %}

### 方式3
c11之徫可以省略等於=
```
struct 結構名{
	變數宣告
}結構變數{};
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
  //學生姓名
  char* name;
  //學號
  int id;
}student{};
int main() {
  return 0;
}
{% endhighlight %}

```
結構  變數名{}
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
  Student student{};
  return 0;
}
{% endhighlight %}

### 方式4
使用memset()
```
memset(結構變數地址,0,sizeof(結構變數));
```

{% highlight c++ linenos %}
int main() {
  Student student;
  memset(&student, 0, sizeof(student));
  return 0;
}
{% endhighlight %}




