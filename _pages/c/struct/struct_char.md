---
title: 結構與字串
date: 2024-07-08
keywords: c++, struct
---

Prerequisites:

- [字串常數][1]

- [字串修改][2]

- [字串指標][3]

## 結構成員與char字串

以下結構成員name為char字串。

{% highlight c++ linenos %}
typedef struct{
    //學生姓名
    char name[100];
    //學號
    int id;
}Student;
{% endhighlight %}

使用char字串，不能使用`等於=`指定值，以下語法將編譯錯誤。

{% highlight c++ linenos %}
student.name = "Mary";
{% endhighlight %}

正確使用的方式是使用strcpy拷貝字串到char字串。

{% highlight c++ linenos %}
strcpy(student.name, "Mary");
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
typedef struct{
    //學生姓名
    char name[100];
    //學號
    int id;
}Student;
int main() {
    Student student;
    //清空資料
    //student記憶體位址的值全設為00000000
    memset(&student, 0, sizeof(student));
    //姓名
    strcpy(student.name, "Mary");
    //學號
    student.id = 100;
    cout << student.name << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

```
Mary
100
```


## 結構成員與char字串指標

以下結構成員name為char字串指標。

{% highlight c++ linenos %}
typedef struct{
    //學生姓名
    char* name;
    //學號
    int id;
}Student;
{% endhighlight %}

使用char字串指標，可以使用`等於=`指定值。

{% highlight c++ linenos %}
student.name = "Mary";
{% endhighlight %}

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
    Student student;
    //清空資料
    //student記憶體位址的值全設為00000000
    memset(&student, 0, sizeof(student));
    //姓名
    student.name = "Mary";
    //學號
    student.id = 100;
    cout << student.name << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

```
Mary
100
```


## 結構成員與char字串與字串指標(c11)

使用大括號的方式，不管是字串或字串指標都可以用雙引號""指定值。

{% highlight c++ linenos %}
student = {"Mary","桃園市XX區XX號", 1};
{% endhighlight %}

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
typedef struct{
    //學生姓名
    char name[100];
    //地址
    char* address;
    //學號
    int id;
}Student;
int main() {
    Student student;
    student = {"Mary","桃園市XX區XX號", 1};
    cout << student.name << endl;
    cout << student.address << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

```
Mary
桃園市XX區XX號
1
```
## 無法使用大括號修改單獨成員是字串陣列

以下的寫法會編譯錯誤

{% highlight c++ linenos %}
student.name = {"Bill"};
{% endhighlight %}

必須使用strcpy修改單獨成員是字串陣列
{% highlight c++ linenos %}
strcpy(student.name, "Bill");
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
typedef struct{
    //學生姓名
    char name[100];
    //地址
    char* address;
    //學號
    int id;
}Student;
int main() {
    Student student;
    student = {"Mary","桃園市XX區XX號", 1};
    strcpy(student.name, "Bill");
    cout << student.name << endl;
    cout << student.address << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

```
Bill
桃園市XX區XX號
1
```

[1]: {% link _pages/c/array/charArray.md %}#字串常數
[2]: {% link _pages/c/array/charArray.md %}#字串修改
[3]: {% link _pages/c/array/pointerCharArr.md %}