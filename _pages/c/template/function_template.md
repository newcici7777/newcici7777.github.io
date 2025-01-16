---
title: 函式模板
date: 2024-12-02
keywords: c++, function template
---
Prerequisites:

- [引數][1]
- [函式多載][2]

函式模板是編譯器根據引數，自動產生出對映的參數函式代碼。

## 模板定義  

由template關鍵字定義模板，模板參數型別定義由關鍵字typename開始，可以命名任何您想要的參數型別，但依照慣例，最常使用單一大寫字母，T 是模板參數型別，若有多個模板參數型別可以T1,T2,T3...作為命名。

```
template <typename T1, typename T2, typename T3>
```

{% highlight c++ linenos %}
// template為模板定義
// typename為模板型別定義
// T 是模板參數
template <typename T>
// 函式模板的名稱為FuncName
// 傳回值void，T為模板參數型別，參數名為param
void FuncName(T param) {
  cout << "param:" << param << endl;
}
{% endhighlight %}

## 函式多載

以下的Swap函式，只有參數的類型不同，屬於函式多載，其它程式碼都一模一樣。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void Swap(int &a, int &b) {
  int temp = a;
  a = b;
  b = temp;
}
void Swap(double &a, double &b) {
  double temp = a;
  a = b;
  b = temp;
}
void Swap(string &a, string &b) {
  string temp = a;
  a = b;
  b = temp;
}
int main() {
  int a = 10, b = 20;
  Swap(a,b);
  cout << "a=" << a << ",b=" << b << endl;
  double d1 = 50.5, d2 = 20.3;
  Swap(d1,d2);
  cout << "d1=" << d1 << ",d2=" << d2 << endl;
  string s1 = "test", s2 = "c++";
  Swap(s1,s2);
  cout << "s1=" << s1 << ",s2=" << s2 << endl;
  return 0;
}
{% endhighlight %}
```
a=20,b=10
d1=20.3,d2=50.5
s1=c++,s2=test
```

## 引數型別替換T模板參數型別

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
template <typename T>
void Swap(T &a, T &b) {
  T temp = a;
  a = b;
  b = temp;
}
int main() {
  int a = 10, b = 20;
  // 函式模板會根據引數的類型自動生成Swap(int, int)對映的代碼
  Swap(a,b);
  cout << "a=" << a << ",b=" << b << endl;
  double d1 = 50.5, d2 = 20.3;
  // 函式模板會根據引數的類型自動生成Swap(double, double)對映的代碼
  Swap(d1,d2);
  cout << "d1=" << d1 << ",d2=" << d2 << endl;
  string s1 = "test", s2 = "c++";
  // 函式模板會根據引數的類型自動生成Swap(string, string)對映的代碼
  Swap(s1,s2);
  cout << "s1=" << s1 << ",s2=" << s2 << endl;
  return 0;
}
{% endhighlight %}
```
a=20,b=10
d1=20.3,d2=50.5
s1=c++,s2=test
```

## 自訂類型

如果不想讓編譯器自動推導類型，想自訂類型，呼叫函式後面加上<類型>，就會產生此類型的函式。

{% highlight c++ linenos %}
template <typename T>
void Swap(T &a, T &b) {
  T temp = a;
  a = b;
  b = temp;
}
int main() {
  int a = 10, b = 20;
  // 自訂類型
  Swap<int>(a,b);
  return 0;
}
{% endhighlight %}

## 傳回值為模板

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
template <typename T>
T Add(T a, T b) {
  return a + b;
}
int main() {
  int a = 10;
  int b = 20;
  int result = Add(a,b);
  cout << "result = " << result << endl;
  return 0;
}
{% endhighlight %}

## 強制轉型

強制把引數轉成欲轉換的型別
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
template <typename T>
T Add(T a, T b) {
  return a + b;
}
int main() {
  int a = 10;
  // 整數類型char
  char c = 'A';
  // 強制轉型char->int
  int result = Add<int>(a,c);
  cout << "result = " << result << endl;
  return 0;
}
{% endhighlight %}
```
result = 75
```

## 模板多載

可以有多個相同函式模板名稱，但模板型別參數的數量不同。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
template <typename T1>
void insertLog(T1 no) {
  cout << "no:" << no << endl;
}
template <typename T1,typename T2>
void insertLog(T1 no,T2 msg) {
  cout << "no:" << no << ",msg:" << msg << endl;
}
template <typename T1,typename T2>
void insertLog(T1 no,T2 msg,int seq) {
  cout << "no:" << no << ",msg:" << msg << ",seq:" << seq << endl;
}
int main() {
  insertLog(10);
  insertLog(11, "insert something...");
  insertLog(12, "insert something...", 1);
  return 0;
}
{% endhighlight %}

```
no:10
no:11,msg:insert something...
no:12,msg:insert something...,seq:1
```

## 引數的類型需要一致

二個引數類型不同會產生會產生"No matching function for call to 'Swap'"的error。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
template <typename T>
void Swap(T &a, T &b) {
  T temp = a;
  a = b;
  b = temp;
}
int main() {
  int a = 10;
  double b = 20.5;
  Swap(a,b);
  return 0;
}
{% endhighlight %}

## 沒有參數的模板

呼叫函式模板Swap，必須自訂類型<int>，否則編譯器無法知道要產生什麼類型的函式模板，錯誤訊息"No matching function for call to 'Swap'"

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
template <typename T>
// Swap模板函式沒有模板類型參數。
void Swap() {
}
int main() {
  Swap<int>();
  return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/basic/param.md %}
[2]: {% link _pages/c/function/func_overload.md %}