---
title: 定義結構
date: 2024-07-03
keywords: c++, struct
---
想要建立只有資料的物件時，使用 struct；其他狀況一律使用 class。<br>

結構是「自訂」(客製化)資料結構類型，大括號`{}`包含這個結構中需要的成員變數，成員變數類型可以是指標、陣列、整數、字元、double、float，也可以是結構。<br>

定義結構有以下三種方式:<br>

## 方式1
### 宣告結構
關鍵字struct宣告，分號於struct後的是結構名稱，變數的宣告都在大括號{}內，大括號{}結尾一定要加上分號;<br>
```
struct 結構名 {
  變數宣告
};
```

{% highlight c++ linenos %}
struct Student {
  // 學生姓名
  char* name;
  // 學號
  int id;
};
{% endhighlight %}

### 結構放置位置
放在main()上面，或者放在標頭檔。

### 定義結構變數
方式一:
```
結構名 變數名;
Student student;
```
{% highlight c++ linenos %}
int main() {
  Student student;
  return 0;
}
{% endhighlight %}

方式二:<br>
前面加上struct。
```
struct 結構名 變數名
struct Student s1;
```
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
struct Student {
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
struct Student {
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

## Struct Memory layout
### 結構記憶體大小
s1變數指向Student結構中第1個成員變數的記憶體位址。<br>
每一個成員變數都有自己的記憶體位址，成員變數name是指標類型，占8byte，id是整數類型，占4byte，addr是指標類型，占8byte。<br>
但結構會以最大的記憶體位址大小 8byte來作為單位。<br>

結構記憶體大小計算:<br>
8byte \* 成員變數的數量3 = 24 byte<br>

所以成員的記憶體位址相差是8byte。<br>

### 結構與字串常數指標
name是指標變數，存放字串常數的記憶體位址，字串常數放置在RODATA唯讀區塊。<br>
id是整數，整數是基本型態，所以直接存放1。<br>
addr是指標變數，存放字串常數的記憶體位址，字串常數放置在RODATA唯讀區塊。<br>

![img]({{site.imgurl}}/c++/struct1.png)<br>

{% highlight c++ linenos %}
struct Student {
  char* name;
  int id;
  char* addr;
};
int main() {
  struct Student s1;
  // 5byte，包含\0字串結尾
  s1.name = "Mary";
  s1.id = 1;
  // 12byte
  s1.addr = "Taiwan,Tapei";
  printf("&s1 = %p, &name = %p, &id = %p, &addr = %p \n",&s1, &s1.name, &s1.id, &s1.addr);
  printf("name = %p, addr = %p \n",s1.name, s1.addr);
  return 0;
}
{% endhighlight %}
```
&s1 = 0x1000, &name = 0x1000, &id = 0x1008, &addr = 0x1010
name = 0x2000, addr = 0x2005
```

### 各自獨立記憶體空間
結構就像是一種模板，用結構宣告的變數s1、變數s2，變數s1與變數s2擁有相同的成員變數名，但變數的記憶體位址是完全不一樣，變數s1與s2有各自獨立記憶體空間。<br>

![img]({{site.imgurl}}/c++/struct2.png)<br>

## 方式2
### 同時宣告結構與定義結構變數
在大括號的結尾輸入結構變數，在main()函式不用定義結構變數，直接使用結構變數。<br>
```
struct 結構名{
	變數宣告
}結構變數;
```
{% highlight c++ linenos %}
struct Student {
  //學生姓名
  char* name;
  //學號
  int id;
}student;
{% endhighlight %}

### 存取結構成員
在main()函式不用定義結構變數，直接使用結構變數。
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
struct Student {
  //學生姓名
  char* name;
  //學號
  int id;
} student;
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
typedef struct {
	變數宣告
} 結構名;
```

{% highlight c++ linenos %}
typedef struct {
  //學生姓名
  char* name;
  //學號
  int id;
} Student;
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
typedef struct {
  //學生姓名
  char* name;
  //學號
  int id;
} Student;
int main() {
  Student student;
  student.name = "Mary";
  student.id = 1;
  cout << student.name << endl;
  cout << student.id << endl;
  return 0;
}
{% endhighlight %}

## 匿名結構
沒有結構名，一次性，只能使用1次，沒有結構名，無法重覆使用。<br>

```
struct{
  成員變數
} 變數;
```

以下程式碼，結構變數s1，是在main()函式中宣告。<br>
struct後面是沒有結構名。<br>
{% highlight c++ linenos %}
int main() {
  struct {
    char* name;
    int id;
    char* addr;
  } s1;
  s1.name = "Mary";
  s1.id = 1;
  s1.addr = "Taiwan,Tapei";
  printf("name = %s, id = %d, addr = %s \n",s1.name, s1.id, s1.addr);
  return 0;
}
{% endhighlight %}
```
name = Mary, id = 1, addr = Taiwan,Tapei 
```