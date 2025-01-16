---
title: bind綁定
date: 2024-11-4
keywords: c++, bind 
---

Prerequisites:

- [function][1]


## 語法

```
function<函式傳回值類型(函式參數1類型 參數名1,函式參數2類型 參數名2)> func2 = bind(函式名, placeholders::_1, placeholders::_2);

function<void(int, const string&)> func2 = bind(print, placeholders::_1, placeholders::_2);
```

placeholders::_1 對映函式參數名1

placeholders::_2 對映函式參數名2


## 函式bind
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
void print(int code, const string& msg) {
  cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
  //設定bind與綁定的參數個數
  function<void(int, const string&)> func = bind(print, placeholders::_1, placeholders::_2);
  //呼叫函式
  func(400, "Page not found.");
  return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```

## 參數對調

以下語法把函式參數對調
<pre>
  function<void(<span class="markline">const string&,int</span>)> func = bind(print,  <span class="markline">placeholders::_2,placeholders::_1</span>);
</pre>

呼叫函式時也要對調
<pre>
	func(<span class="markline">"Page not found.", 400</span>);
</pre>

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
void print(int code, const string& msg) {
  cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
  //設定bind與綁定的參數個數
  function<void(const string&,int)> func = bind(print,  placeholders::_2,placeholders::_1);
  //呼叫函式
  func("Page not found.", 400);
  return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```

## 參數數量可自訂

使用bind，可以預設參數值，限制傳進參數的個數。
<pre>
  function<void(<span class="markline">const string&</span>)> func = bind(print, <span class="markline">400, placeholders::_1</span>);
</pre>

呼叫函式只傳一個參數

<pre>
func(<span class="markline">"Page not found."</span>);
</pre>

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
void print(int code, const string& msg) {
  cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
  //設定bind與綁定的參數個數
  function<void(const string&)> func = bind(print, 400, placeholders::_1);
  //呼叫函式
  func("Page not found.");
  return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```
## bind預設傳值

以下的程式碼，即便之後把error_code改成500，顯示仍為400

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
void print(int code, const string& msg) {
  cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
  int error_code = 400;
  //設定bind與綁定的參數個數
  function<void(const string&)> func = bind(print, error_code, placeholders::_1);
  //修改成500
  error_code = 500;
  //呼叫函式
  func("Page not found.");
  return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```

## bind傳參考

語法
<pre>
function<void(const string&)> func = bind(print, <span class="markline">ref(error_code)</span>, placeholders::_1);
</pre>

使用ref(), 可以傳參考，也就可以修改變數中的值。

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
void print(int code, const string& msg) {
  cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
  int error_code = 400;
  //設定bind與綁定的參數個數
  function<void(const string&)> func = bind(print, ref(error_code), placeholders::_1);
  error_code = 500;
  //呼叫函式
  func("Page not found.");
  return 0;
}
{% endhighlight %}
```
Error code = 500 , Msg = Page not found.
```

## 參數傳的比函式參數還多

print函式只有2個參數，但有一個需求需要傳3個參數，怎麼修改？

在function的函式參數多增加一個，呼叫函式的參數也多增加一個

<pre>
  function<void(int, const string&, <span class="markline">int</span>)> func = bind(print, placeholders::_1, placeholders::_2);
  func(400, "Page not found.", <span class="markline">1000</span>);
</pre>

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
void print(int code, const string& msg) {
  cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
  //設定bind與綁定的參數個數
  function<void(int, const string&, int)> func = bind(print, placeholders::_1, placeholders::_2);
  //呼叫函式
  func(400, "Page not found.", 1000);
  return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```

## 物件成員函式與bind

- function<參數類型1, ...> 第1個參數要填入類別參考&
- 等於號(=)指派對映的函式，必須把記憶體位址傳進去，所以使用&取位址運算子+類別名+::範圍運算子+函式名
- 注意！bind的參數有三個，分別是物件參考student&，整數int，字串參考string&
<pre>
  function<void(<span class="markline">Student&</span>,int, const string&)> func =  bind(<span class="markline">&Student::print</span>, <span class="markline">placeholders::_1</span>, <span class="markline">placeholders::_2</span>, <span class="markline">placeholders::_3</span>);
</pre>

### 呼叫function

必須把物件代入第1個參數

<pre>
  func(<span class="markline">student</span>, 500, "Server error.");
</pre>

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
class Student {
public:
  void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
  }
};
int main() {
  //建立物件
  Student student;
  //設定bind與綁定的參數個數
  function<void(Student&,int, const string&)> func = bind(&Student::print, placeholders::_1, placeholders::_2,placeholders::_3);
  //呼叫函式
  func(student, 400, "Page not found.");
  return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```


## 類別靜態函式

### 語法

只需要函式前面加上類別名::
<pre>
function<void(int, const string&)> func = bind(<span class="markline">Student::</span>print, placeholders::_1, placeholders::_2);
</pre>


### 完整程式碼
{% highlight c++ linenos %}
class Student {
public:
  static void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
  }
};
int main() {
  //設定bind與綁定的參數個數
  function<void(int, const string&)> func = bind(Student::print, placeholders::_1, placeholders::_2);
  //呼叫函式
  func(400, "Page not found.");
  return 0;
}
{% endhighlight %}

## lambda

bind的第1個參數代入lambda的變數名，也可以直接把lambda函式放進bind的第一個參數。

{% highlight c++ linenos %}
int main() {
  //lambda
  auto print = [](int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
  };
  //設定bind與綁定的參數個數
  function<void(int, const string&)> func = bind(print, placeholders::_1, placeholders::_2);
  //呼叫函式
  func(400, "Page not found.");
  return 0;
}
{% endhighlight %}


## 物件函式

bind的第1個參數代入student物件。

{% highlight c++ linenos %}
class Student {
public:
  void operator()(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
  }
};
int main() {
  //建立物件
  Student student;
  //設定bind與綁定的參數個數
  function<void(int, const string&)> func = bind(student, placeholders::_1, placeholders::_2);
  //呼叫函式
  func(400, "Page not found.");
  return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```

[1]: {% link _pages/c/c11/functional.md %}