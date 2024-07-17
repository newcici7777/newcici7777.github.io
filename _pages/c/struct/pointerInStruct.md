---
title: 結構中的指標
date: 2024-07-15
keywords: c++, pointer within a Structure
---

Prerequisites:

- [new/delete][1]
- [null 記憶體區間][2]

## memset與結構中的指標

要把結構中的指標釋放記憶體，光是把結構使用memset把結構記憶體的值全設為00000000是不夠的，因為只是把指標指向的位置指到00000000([null 記憶體區間][2])，原本指標指向的位置的值仍未清空成0，會造成記憶體洩露。

### 錯誤作法

```
 memset(&student, 0, sizeof(student));
```

以下為錯誤作法
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
const int SCORE_MAX = 3;
struct Student{
    //學生姓名
    char name[100];
    //學號
    int id;
    //結構中的指標
    int *score;
};
int main() {
    Student student;
    strcpy(student.name, "Mary");
    student.id = 1;
    student.score = new int[SCORE_MAX];
    
    for(int i = 0; i < SCORE_MAX; i++) {
        student.score[i] = i;
    }
    //100 byte(name) + 4 byte(id) + 8 byte(*score指標)
    //總共占 112 byte記憶體大小
    cout << "結構大小 = " << sizeof(student) << endl;
    //印出score指標指向的記憶體位址
    cout << "結構成員指標指向的記憶體位址 = " <<  student.score << endl;
    //將student.score的記憶體位址先暫存下來
    int* pp = student.score;
    //將結構的成員全清成00000000
    memset(&student, 0, sizeof(student));
    cout << "After memset :" << endl;
    //印出score指標指向的記憶體位址
    cout << "結構成員指標指向的記憶體位址 = " << student.score << endl;
    cout << "原結構成員指標指向的記憶體位址 = " << pp << endl;
    //原結構成員指標指向的記憶體位址中的值
    for(int i = 0; i < SCORE_MAX; i++) {
        cout << "student.score[" << i << "] = " << *(pp + i) << endl;
    }
    return 0;
}
{% endhighlight %}
```
結構大小 = 112
結構成員指標指向的記憶體位址 = 0x600000008040
After memset :
結構成員指標指向的記憶體位址 = 0x0
原結構成員指標指向的記憶體位址 = 0x600000008040
student.score[0] = 0
student.score[1] = 1
student.score[2] = 2
```

### 正確作法
```
memset(student.score, 0, sizeof(int)*SCORE_MAX);
```

memset第3個參數不可以寫成成員指標，因為指標的記憶體大小永遠是8。

以下是錯誤寫法
```
memset(student.score, 0, sizeof(student.score));
```

以下的作法才是真正做到結構中指標的記憶體釋放。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
const int SCORE_MAX = 3;
struct Student{
    //學生姓名
    char name[100];
    //學號
    int id;
    //結構中的指標
    int *score;
};
int main() {
    Student student;
    strcpy(student.name, "Mary");
    student.id = 1;
    student.score = new int[SCORE_MAX];
    
    for(int i = 0; i < SCORE_MAX; i++) {
        student.score[i] = i;
    }
    //100 byte(name) + 4 byte(id) + 8 byte(*score指標)
    //總共占 112 byte記憶體大小
    cout << "結構大小 = " << sizeof(student) << endl;
    //印出score指標指向的記憶體位址
    cout << "結構成員指標指向的記憶體位址 = " <<  student.score << endl;
    //將student.score的記憶體位址先暫存下來
    int* pp = student.score;
    //將結構的成員指向的記憶體位址的值全清成00000000
    memset(student.score, 0, sizeof(int)*SCORE_MAX);
    //將結構的成員全清成00000000
    memset(&student, 0, sizeof(student));
    cout << "After memset :" << endl;
    //印出score指標指向的記憶體位址
    cout << "原score成員的記憶體位址 = " << *pp << endl;
    cout << "score成員的記憶體位址 = " << student.score << endl;
    return 0;
}
{% endhighlight %}
```
結構大小 = 112
結構成員指標指向的記憶體位址 = 0x600000004060
After memset :
原score成員的記憶體位址 = 0
score成員的記憶體位址 = 0x0
```

## 結構中的指標是自己

注意看結構Student中的指標指向的是Student結構，也就是結構中的指標是自已。
{% highlight c++ linenos %}
struct Student{
    //學生姓名
    char name[100];
    //學號
    int id;
    //結構中的指標
    Student *next;
};
{% endhighlight %}

不能寫成以下的寫法，會編譯失敗。

{% highlight c++ linenos %}
struct Student{
    //學生姓名
    char name[100];
    //學號
    int id;
    //結構中的指標
    Student next;
};
{% endhighlight %}

以上設計這種結構是表示每一個學生都有一個指標，指向下一個學生。

學生A->學生B->學生C->學生D->null

### 初始化
初始化有三種方法

#### 方式一

{% highlight c++ linenos %}
    Student *student = new Student;
    strcpy(student->name, "Mary");
    student->id = 1;
    student->next = nullptr;
{% endhighlight %}

#### 方式二
{% highlight c++ linenos %}
    Student *student = new Student({ "Mary", 1, nullptr});
{% endhighlight %}


{% highlight c++ linenos %}
    Student *student = nullptr; 
    student = new Student({ "Mary", 1, nullptr});
{% endhighlight %}

#### 方式三
{% highlight c++ linenos %}
    Student *student;
    *(student) = {1, "Mary", nullptr};
{% endhighlight %}


完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
    //學生姓名
    char name[100];
    //學號
    int id;
    //結構中的指標
    Student *next;
};
int main() {
    Student *student = new Student({ "Mary", 1, nullptr});
    return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/dynamicMemory/newDelete.md %}
[2]: {% link _pages/c/pointer/nullptr.md %}#nullptr記憶體位址
