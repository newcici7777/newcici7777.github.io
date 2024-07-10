---
title: 結構陣列
date: 2024-07-08
keywords: c++, struct
---

Prerequisites:

- [陣列清空][1]

- [陣列運算][2]

## 結構陣列的宣告方式

```
結構 陣列名[陣列大小];
Student students[size];
```

## 結構陣列指定元素

### 使用.運算子

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
    int size = 3;
    //建立三個學生
    Student students[size];
    //清空資料
    //參數1是地址，student陣列名就是陣列記憶體的開始位址
    memset(students, 0, sizeof(students));
    
    strcpy(students[0].name,"Bill");
    students[0].id = 1;
    
    strcpy(students[1].name,"Mary");
    students[1].id = 2;
    
    strcpy(students[2].name,"Jeff");
    students[2].id = 3;
    
    for(int i = 0; i < size; i++) {
        cout << "id = " << students[i].id;
        cout << ", name = " << students[i].name << endl;
    }
    return 0;
}
{% endhighlight %}

```
id = 1, name = Bill
id = 2, name = Mary
id = 3, name = Jeff
```

### 使用大括號{}

C++11以上才能使用。

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
    int size = 3;
    //建立三個學生
    Student students[size];
    //清空資料
    //參數1是地址，student陣列名就是陣列記憶體的開始位址
    memset(students, 0, sizeof(students));
    students[0] = {"Bill",1};
    students[1] = {"Mary",2};
    students[2] = {"Jeff",3};
    
    for(int i = 0; i < size; i++) {
        cout << "id = " << students[i].id;
        cout << ", name = " << students[i].name << endl;
    }
    return 0;
}
{% endhighlight %}

```
id = 1, name = Bill
id = 2, name = Mary
id = 3, name = Jeff
```

## 使用指標運算取值

使用箭頭->方式取值，括號()前面不需要加上星號\*

```
(陣列名 + 元素索引index)->結構成
```

{% highlight c++ linenos %}
(students+0)->id
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
    int size = 3;
    //建立三個學生
    Student students[size];
    //清空資料
    //參數1是地址，student陣列名就是陣列記憶體的開始位址
    memset(students, 0, sizeof(students));
    students[0] = {"Bill",1};
    students[1] = {"Mary",2};
    students[2] = {"Jeff",3};
    
    cout << "id = " << (students+0)->id;
    cout << ", name = " << (students+0)->name << endl;
    
    for(int i = 0; i < size; i++) {
        cout << "id = " << (students+i)->id;
        cout << ", name = " << (students+i)->name << endl;
    }
    return 0;
}
{% endhighlight %}

```
id = 1, name = Bill
id = 1, name = Bill
id = 2, name = Mary
id = 3, name = Jeff
```

## 使用指標運算指定值

```
*(陣列名 + 元素索引index) = {值1,值2, ...};
```

{% highlight c++ linenos %}
    *(students + 0) = {"Bill",1};
    *(students + 1) = {"Mary",2};
    *(students + 2) = {"Jeff",3};
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
    int size = 3;
    //建立三個學生
    Student students[size];
    //清空資料
    //參數1是地址，student陣列名就是陣列記憶體的開始位址
    memset(students, 0, sizeof(students));
    *(students + 0) = {"Bill",1};
    *(students + 1) = {"Mary",2};
    *(students + 2) = {"Jeff",3};
    
    cout << "id = " << (students+0)->id;
    cout << ", name = " << (students+0)->name << endl;
    
    for(int i = 0; i < size; i++) {
        cout << "id = " << (students+i)->id;
        cout << ", name = " << (students+i)->name << endl;
    }
    return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/array/array.md %}#memset陣列清空
[2]: {% link _pages/c/array/pointerToArray.md %}#陣列名--1