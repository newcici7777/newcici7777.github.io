---
title: 定義結構
date: 2024-07-03
keywords: c++, struct
---

結構是變數的集合，變數可以不是不同型態，例如有的是int，有的是float，結構中每一個變數都是該結構的成員。

定義結構有以下三種方式

## 方式1

### 宣告結構

關鍵字struct宣告，緊號於struct後的是結構名稱，變數的宣告都在大括號{}內，大括號{}結尾一定要加上分號;

```
struct 結構名{
	變數宣告
};
```

{% highlight c++ linenos %}
struct Student{
    //學生姓名
    char* name;
    //學號
    int id;
};
{% endhighlight %}

### 結構放置位置

放在main()上面，或者放在標頭檔。

### 定義結構變數

結構可想像成一種新的資料型態，int也是一種資料型態，定義結構變數會在記憶體配置空間。

```
結構名 變數名;
```

{% highlight c++ linenos %}
int main() {
    Student student;
    return 0;
}
{% endhighlight %}

### 存取結構成員

存取方式有二種

#### 點運算子

使用點運算子(dot operator)指派或修改結構成員的值。

{% highlight c++ linenos %}
    Student student;
    student.name = "Mary";
    student.id = 1;
{% endhighlight %}

cout讀取結構成員。

{% highlight c++ linenos %}
    cout << student.name << endl;
    cout << student.id << endl;
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
    //學生姓名
    char* name;
    //學號
    int id;
};
int main() {
    Student student;
    student.name = "Mary";
    student.id = 1;
    cout << student.name << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

```
Mary
1
```

#### 大括號

使用大括號修改結構。

```
結構名 = {值1,值2,值3, ...};
```

{% highlight c++ linenos %}
student = {"Bill",2};
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
    //學生姓名
    char* name;
    //學號
    int id;
};
int main() {
    Student student;
    student.name = "Mary";
    student.id = 1;
    cout << "Before:" << endl;
    cout << student.name << endl;
    cout << student.id << endl;
    student = {"Bill",2};
    cout << "After:" << endl;
    cout << student.name << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

## 方式2

### 同時宣告結構與定義結構變數

在大括號的結尾輸入結構變數，在main()函數不用定義結構變數，直接使用結構變數。

```
struct 結構名{
	變數宣告
}結構變數;
```
{% highlight c++ linenos %}
struct Student{
    //學生姓名
    char* name;
    //學號
    int id;
}student;
{% endhighlight %}

### 存取結構成員

在main()函數不用定義結構變數，直接使用結構變數。

{% highlight c++ linenos %}
int main() {
    student.name = "Mary";
    student.id = 1;
    cout << student.name << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
    //學生姓名
    char* name;
    //學號
    int id;
}student;
int main() {
    student.name = "Mary";
    student.id = 1;
    cout << student.name << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}

## 方式3

### 宣告結構

使用typedef(型別定義)關鍵字，後面緊跟著struct關鍵字，再來是大括號{}，變數的宣告在大括號{}中，結構名放在大括號結尾，最後加上分號;結束。

```
typedef struct{
	變數宣告
}結構名;
```

{% highlight c++ linenos %}
typedef struct{
    //學生姓名
    char* name;
    //學號
    int id;
}Student;
{% endhighlight %}

### 定義結構變數

```
結構名 變數名;
```

{% highlight c++ linenos %}
Student student;
{% endhighlight %}

### 存取結構成員

與[方式一](#存取結構成員)相同。

### 完整程式碼

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
    student.name = "Mary";
    student.id = 1;
    cout << student.name << endl;
    cout << student.id << endl;
    return 0;
}
{% endhighlight %}
