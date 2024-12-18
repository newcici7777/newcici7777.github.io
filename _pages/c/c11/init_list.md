---
title: 初始化initializer list
date: 2024-12-06
keywords: c++, initializer list
---

Prerequisites:

[維基介紹 初始化串列][1]

## 整數

c++11用大括號{},等於號可省略
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  int a = 10;
  // c++98,用圓括號()
  int b = (10);
  int c(10);
  cout << "a = " << a << endl;
  cout << "b = " << b << endl;
  cout << "c = " << c << endl;
  
  //c++11用大括號{},等於號可省略
  int d = {10};
  int e{10};
  cout << "d = " << d << endl;
  cout << "e = " << e << endl;
  return 0;
}
{% endhighlight %}
```
a = 10
b = 10
c = 10
d = 10
e = 10
```

可以用 =、() 或 {}，以下用法都對

{% highlight c++ linenos %}
int x = 3;
int x(3);
int x{3};
string name("Some Name");
string name = "Some Name";
string name{"Some Name"};
{% endhighlight %}

## 浮點數

{% highlight c++ linenos %}
  double f{1.12};
  cout << "f = " << f << endl;
{% endhighlight %}
```
f = 1.12
```

此外，{} 初始列不允許整數型別的縮小 (narrowing) 轉換，這可以用來避免一些型別上的程式撰寫錯誤。

{% highlight c++ linenos %}
int pi(3.14);  // 可 -- pi == 3.
int pi{3.14};  // 編譯器錯誤：縮小轉換
{% endhighlight %}

## 陣列
{% highlight c++ linenos %}
  //陣列
  int arr1[5]{1, 2, 3, 4, 5};
  for (int i = 0; i < 5; i++) {
    cout << "arr1[" << i << "] = " << arr1[i] << endl;
  }
{% endhighlight %}
```
arr1[0] = 1
arr1[1] = 2
arr1[2] = 3
arr1[3] = 4
arr1[4] = 5
```

## new動態分配記憶體位址
{% highlight c++ linenos %}
  int *arr_p = new int[5]{11, 12, 13, 14, 15};
  for (int i = 0; i < 5; i++) {
    cout << "arr_p[" << i << "] = " << arr_p[i] << endl;
  }
{% endhighlight %}
```
arr_p[0] = 11
arr_p[1] = 12
arr_p[2] = 13
arr_p[3] = 14
arr_p[4] = 15
```

## 建立物件

c++11用大括號{},等於號可省略
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
 public:
  Student(int number, string name) : number(number), name(name){}
 private:
  int number;  // 學號
  string name;
};
int main() {
  // c98
  Student s1(10122, "Bill");
  // c11
  Student s2 = {10123, "Mary"};
  //c11 省略等於號
  Student s3{10124, "Alice"};
  return 0;
}
{% endhighlight %}

## 初始化容器
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
int main() {
  // 把v1初始化為10個元素
  vector<int> v1(10);
  // v2只有1個元素10
  vector<int> v2{10};
  // v3有3個元素，分別是11 12 13
  vector<int> v3{11, 12, 13};
  return 0;
}
{% endhighlight %}

{% highlight c++ linenos %}
std::vector<int> v(100, 1);  // vector 中有 100 個元素：每個元素都是 1
std::vector<int> v{100, 1};  // vector 中有 2 個元素：100 和 1
{% endhighlight %}

## 函式參數
可以把大括號{}間的值，作為參數傳給函式。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int func1(initializer_list<int> init_list) {
  int total = 0;
  for (int val : init_list) {
    total += val;
    cout << val << ",";
  }
  cout << endl;
  return total;
}
int main() {
  int total = func1({1, 2, 3, 4, 5});
  cout << "total = " << total << endl;
  return 0;
}
{% endhighlight %}
```
1,2,3,4,5,
total = 15
```

- [iterator][2]

initializer_list模板類別，跟容器一樣，提供begin()與end()

## 編譯

- [makefile][3]

若在linux編譯時，需要加上-std=c++11
```
g++ -o c11_init c11_init.cpp -std=c++11
```


[1]: https://zh.wikipedia.org/zh-tw/C%2B%2B11
[2]: {% link _pages/c/stl/iterator.md %}
[3]: {% link _pages/c/compile/makefile.md %}