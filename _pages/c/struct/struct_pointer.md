---
title: 指標指向結構與結構陣列
date: 2024-07-03
keywords: c++, struct
---
## 位址運算子優先等級
由下表可以知道\*與&優先等級比結構、陣列相關的運算子低。

|優先等級|運算元|
|1      |() 陣列[] 結構相關. -> |
|2      |記憶體位址相關 \* &|

## 結構與位址運算子優先等級
Student結構如下:<br>
{% highlight c++ linenos %}
struct Student{
  const char* name;
  int id;
  const char* addr;
}
{% endhighlight %}

在main()函式空間建立Student結構，並建立指標指向s1。<br>
{% highlight c++ linenos %}
int main() {
  struct Student{
    const char* name;
    int id;
    const char* addr;
  } s1 = {"Mary", 1, "Taiwan"};
  Student* p = &s1;
  return 0;
}
{% endhighlight %}

使用指標試圖存取s1的name，以下的寫法，會先取得s1.name，再對\* (s1.name)記憶體位址使用取值運算子。因為先前表格已經說明`.`點是優先等級1。<br>

以下程式寫法是相同的。<br>
{% highlight c++ linenos %}
  cout << * p.name << endl;
  cout << * (p.name) << endl;
{% endhighlight %}

但p指標，並不支援`.`，況且p指標是記憶位址，它也沒有name成員變數。<br>
以下編譯錯誤。<br>
```
p.name
```

要使用`.`存取結構的成員變數，要先把p指標使用\*取值運算子，之前在指標的文章中已提過，要修改記憶體存放的值，要使用\*取值運算子。
```
*指標 = 修改的內容
```

1. 先對記憶體位址，使用\*取值運算子把結構從記憶體取出來。<br>
2. 才能對結構使用`.`存取成員變數。<br>

記得上面的順序嗎？但運算子優先順序表，`.`的優先順序比\*取值運算子還高，所以要使用圓括號`()`，先把結構從記憶體取出來。<br>
{% highlight c++ linenos %}
cout << (* p).name << endl;
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
int main() {
  struct Student{
    const char* name;
    int id;
    const char* addr;
  } s1 = {"Mary", 1, "Taiwan"};
  Student* p = &s1;
  cout << (* p).name << endl;
  return 0;
}
{% endhighlight %}

## 結構指標->成員變數
因為`.`結構成員變數存取運算子與\*取值運算子有優先順序問題，因此又發明了`->`結構「指標」存取結構成員變數運算子。

它取代了以下步驟:<br>
1. 對指標(記憶體位址)使用\*取值運算子，從記憶體中取出結構。
2. 使用`.`運算子，存取結構成員變數。

以下步驟完全取代`(* p).name`。注意，以下語法不必在指標前面加上\*。
{% highlight c++ linenos %}
cout << p->name << endl;
{% endhighlight %}

## 指標指向結構陣列與.
{% highlight c++ linenos %}
int main() {
  // Student結構
  struct Student{
    const char* name;
    int id;
    const char* addr;
  };
  // 結構陣列
  Student arr[2] = {
    {"Mary", 1 ,"Taipei"},
    {"Bill", 2 ,"Taoyuan"}
  };

  // 指標指向結構陣列
  Student* p = arr;
  
  // 取得索引[1]的結構，並取得name成員變數
  cout << (* (p + 1)).name << endl;
  return 0;
}
{% endhighlight %}

拆解以下這句程式碼<br>
{% highlight c++ linenos %}
  cout << (* (p + 1)).name << endl;
{% endhighlight %}

1.指標一開始是指向陣列[0]的記憶體位址。<br>
{% highlight c++ linenos %}
Student* p = arr;
{% endhighlight %}

2.指標移動24byte，Student結構記憶體大小為每個成員變數大小8byte \* 成員變數數量3 = 24byte，這個部分之前在結構中的記憶體大小計算已提過。<br>
{% highlight c++ linenos %}
p + 1
{% endhighlight %}

3.使用\*取值運算子，把結構從記憶體取出來。
{% highlight c++ linenos %}
* (p + 1)
{% endhighlight %}

4.使用`.`存取成員變數，注意`.`優先級高於\*，以下的寫法會先執行`(p + 1).name`對指標使用結構`.`存取成員變數，但指標不會有成員變數，編譯錯誤。<br>
{% highlight c++ linenos %}
* (p + 1).name
{% endhighlight %}

對指標(記憶體位址)使用\*取值運算子，結構從記憶體中取出來，再對結構使用`.`存取成員變數。<br>
{% highlight c++ linenos %}
(* (p + 1)).name
{% endhighlight %}

以上這一切太麻煩了，因此C語言發明了->，讓指標可以存取結構的成員變數。<br>
```
結構指標->成員變數
```

以下程式碼等同`(* (p + 1)).name`。<br>
使用圓括號`(p + 1)`是因為\+的運算子優先順序遠低於->，再看一次最前面的表格，優先次序最高是存取結構的`.`與->。
{% highlight c++ linenos %}
(p + 1) -> name
{% endhighlight %}

以下是舊的文章內容
---------------

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
  Student* ptr = &student;
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
  Student* ptr = &student;
  cout << "姓名 : " << ptr->name << endl;
  cout << "學號 : " << ptr->id << endl;
  return 0;
}
{% endhighlight %}
