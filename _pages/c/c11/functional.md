---
title: functional
date: 2024-11-01
keywords: c++, functional
---
Prerequisites:

- [typedef類型別名][1]
- [函式宣告與定義][2]
- [typedef函式指標類型別名][3]
- [函式參考][4]
- [using函式指標別名][5]


以上文章必須看完，才能往下看。

大陸稱包裝器，台灣目前不知稱為什麼。

功能類似函式指標


## include

```
#include <functional>
```

## 語法1

宣告function

```
function<函式回傳值類型(函式參數1類型 參數名1,函式參數2類型 參數名2, ...)> 變數名 = 函式名;
```

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
//函式定義
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
    //宣告function
    function<void(int, const string&)> func = print;
    //呼叫函式
    func(500, "Server error.");
    return 0;
}
{% endhighlight %}

```
Error code = 500 , Msg = Server error.
```

## 語法2

可以把函式類型取別名

```
using FuncType = void(int, const string&);
```

宣告function時，在模板類型設成FuncType函式類型別名
```
function<FuncType> func = print;
```

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
//函式定義
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
    using FuncType = void(int, const string&);
    //宣告function
    function<FuncType> func = print;
    //呼叫函式
    func(500, "Server error.");
    return 0;
}
{% endhighlight %}

```
Error code = 500 , Msg = Server error.
```

## 類別靜態函式指標

語法
<pre>
function<void(int, const string&)> func =  <span class="markline">Student::</span>print;
</pre>


{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
class Student {
public:
    static void print(int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    }
};
int main() {
    //宣告function
    function<void(int, const string&)> func = Student::print;
    //呼叫函式
    func(500, "Server error.");
    return 0;
}
{% endhighlight %}

## 物件函式

- [物件函式][6]

語法
<pre>
function<函式回傳值類型(函式參數1類型,函式參數2類型, ...)> func =  <span class="markline">物件名</span>;
</pre>

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
class Student {
public:
    void operator()(int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    }
};
int main() {
    //建立物件
    Student student;
    //宣告function
    function<void(int, const string&)> func = student;
    //呼叫函式
    func(500, "Server error.");
    return 0;
}
{% endhighlight %}

## lambda

- [lambda][7]

### 程式碼1
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
int main() {
    //lambda
    auto print = [](int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    };
    //宣告function
    function<void(int, const string&)> func = print;
    //呼叫函式
    func(500, "Server error.");
    return 0;
}
{% endhighlight %}

### 程式碼2

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
int main() {
    //宣告function
    function<void(int, const string&)> func = [](int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    };
    //呼叫函式
    func(500, "Server error.");
    return 0;
}
{% endhighlight %}

## 物件成員函式

### 宣告function

- function<參數類型1, ...> 第1個參數要填入類別參考&
- 等於號(=)指派對映的函式，必須把記憶體位址傳進去，所以使用&取位址運算子+類別名+::範圍運算子+函式名
<pre>
    function<void(<span class="markline">Student&</span>,int, const string&)> func = <span class="markline">&Student::</span>print;
</pre>

### 呼叫function

必須把物件代入第1個參數

<pre>
    func(<span class="markline">student</span>, 500, "Server error.");
</pre>

### 完整程式碼
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
    //宣告function
    function<void(Student&,int, const string&)> func = &Student::print;
    //呼叫函式
    func(student, 500, "Server error.");
    return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/basic/typedef.md %}
[2]: {% link _pages/c/function/func_def.md %}
[3]: {% link _pages/c/function/functionPointer.md %}#typedef函式指標類型別名
[4]: {% link _pages/c/function/func_ref.md %}
[5]: {% link _pages/c/function/using_func_pointer.md %}
[6]: {% link _pages/c/class/functor.md %}
[7]: {% link _pages/c/function/lambda.md %}