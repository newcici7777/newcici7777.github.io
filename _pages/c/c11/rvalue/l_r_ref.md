---
title: l-value參考與r-value參考
date: 2024-06-24
keywords: c++, l-value reference, r-value reference
---
Prerequisites:

- [l-value與r-value][1]

## l-value參考

l-value參考指向變數

{% highlight c++ linenos %}
int main() {
  int i = 10;
  int& l_ref = i;
  cout << "l-value ref = " << l_ref << endl;
  cout << "=== modify ref === " << endl;
  l_ref = 55;
  cout << "l-value ref = " << l_ref << endl;
  return 0;
}
{% endhighlight %}

```
l-value ref = 10
=== modify ref === 
l-value ref = 55
```

## r-value參考

### 幫右值取名字

語法
```
右值資料型態&& 右值變數名 = 右值;
int&& r_ref = 100;
```

r-value參考指向數字

{% highlight c++ linenos %}
int main() {
  int&& r_ref = 100;
  cout << "r-value ref = " << r_ref << endl;
  cout << "=== modify ref === " << endl;
  r_ref = 1000;
  cout << "r-value ref = " << r_ref << endl;
  return 0;
}
{% endhighlight %}

```
r-value ref = 100
=== modify ref === 
r-value ref = 1000
```
### 取出右值記憶體位址

右值取名字後，`名字`就變成左值，就可以取出記憶體位址。

{% highlight c++ linenos %}
int main() {
  int&& r = 10;
  cout << "r address = " << &r << endl;
  return 0;
}
{% endhighlight %}
```
r address = 0x7ff7bfeff45c
```

### 修改右值

右值取名字後，`名字`就變成左值，可以修改值。
{% highlight c++ linenos %}
int main() {
  //r-value
  int&& r = 10;
  cout << "Before r = " << r << endl;
  r = 20;
  cout << "After r = " << r << endl;
{% endhighlight %}
```
Before r = 10
After r = 20
```

### 左值不能放在右值參考

以下程式碼會編譯錯誤

{% highlight c++ linenos %}
int main() {
  //l_val是l-value
  int l_val = 10;
  //定義右值參考
  int&& r_val_ref = l_val;
  return 0;
}
{% endhighlight %}

### 右值不能放在左值參考

以下程式碼會編譯錯誤

{% highlight c++ linenos %}
int main() {
  // 10 is r-value
  int& l_val_ref = 10;
  return 0;
}
{% endhighlight %}

## 函式參數為l-value參考

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int& l_ref) {
  l_ref = 20;
}
int main() {
  int i = 10;
  cout << "before i = " << i << endl;
  func(i);
  cout << "after i = " << i << endl;
  return 0;
}
{% endhighlight %}

```
before i = 10
after i = 20
```
## 函式參數為r-value參考

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int&& r_ref) {
  cout << "r-value = " << r_ref << endl;
}
int main() {
  func(1000);
  return 0;
}
{% endhighlight %}

```
r-value = 1000
```

## 引數與參數r-value參考資料型態不同

[引數][2]與參數的資料型態不同，若自動轉型可以轉成功，也可以使用r-value參考。

{% highlight c++ linenos %}
void func1(int&& r_ref) {
  cout << "r-value = " << r_ref << endl;
}
int main() {
  func1('B');
  return 0;
}
{% endhighlight %}

```
r-value = 66
```

## 函式傳回值為右值

- [臨時物件][3]

語法
```
類別名&& 變數名 = 函式();
Student&& s1 = getStudent1();
```
把右值取了名字，這個名字就是左值，可以做左值能做的事，比如存取物件成員變數，呼叫成員函式。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  string name;
};
Student getStudent1() {
  Student s;
  s.name = "Bill";
  return s;
}
Student getStudent2() {
  //臨時物件
  return Student();
}
int main() {
  Student&& s1 = getStudent1();
  cout << "s1 name = " << s1.name << endl;
  Student&& s2 = getStudent2();
  s2.name = "Mary";
  cout << "s2 name = " << s2.name << endl;
  return 0;
}
{% endhighlight %}
```
s1 name = Bill
s2 name = Mary
```


[1]: {% link _pages/c/c11/rvalue/l_r_value.md %}
[2]: {% link _pages/c/basic/param.md %}
[3]: {% link _pages/c/class/temp_obj.md%}