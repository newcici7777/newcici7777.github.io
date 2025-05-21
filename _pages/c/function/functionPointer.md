---
title: 函式指標
date: 2024-06-03
keywords: c++, pointer, function pointer
---

函式指標指向函式的記憶體位址，通過函式指標使用函式。

### 函式資料型態

函式資料型態是由函式的`傳回值資料型態/參數資料型態/參數的數量`所組成。

如果多個函式的`傳回值資料型態/參數資料型態/參數的數量`都一樣，代表這些函式是同一個資料型態。

以下三個函式都是相同資料型態，傳回值資料型態/參數資料型態/參數的數量都一樣。

* int func1(int code, string msg);
* int func2(int age, string name);
* int func3(int userId, string userName);


以下三個函式都是相同資料型態，傳回值資料型態/參數資料型態/參數的數量都一樣。

* bool func4(string param1);
* bool func5(string msg);
* bool func6(string name);

以下二個函式不是相同資料型態。

* int func1(int code, string msg);
* bool func4(string param1);


以下三個函式的函式資料型態為 int (\*pf1)(int,string)，其中pf1為函式指標的變數名，可以為任意名稱，名稱前面要有星號\*，要用括號()包起來，int為傳回值資料型態，(int,string)為函式參數資料型態與參數數量。

* int func1(int code, string msg);
* int func2(int age, string name);
* int func3(int userId, string userName);

以下三個函式的函式資料型態為 bool (\*pf2)(string)，其中pf2為函式指標的變數名，可以為任意名稱。

* bool func4(string param1);
* bool func5(string msg);
* bool func6(string name);

### 宣告函式指標

{% highlight c++ linenos %}
int func1(int code, string msg) {
  cout << "Err code = " << code << ", msg = " << msg << endl;
  return code;
}
int main() {
  int (*pf1)(int,string); //宣告函式指標變數pf1
  pf1 = func1; //將func1函式設給pf1函式指標變數
  pf1(404, "Page not found."); //使用函式指標pf1呼叫函式
  return 0;
}
{% endhighlight %}

第1行,宣告函式。

第6行,宣告函式指標變數pf1。

第7行,將func1函式設給pf1函式指標變數。

第8行,使用函式指標pf1呼叫函式。

```
執行結果
Err code = 404, msg = Page not found.
```

### typedef函式指標類型別名

- [typedef類型別名][1]

語法

```
typedef 傳回值(*類型別名)(參數1,參數2,參數3 ...);
```

將前一個宣告函式指標的程式碼
{% highlight c++ linenos %}
int (*pf1)(int,string); 
{% endhighlight %}

改成下方的程式碼，前面添加typedef，並把pf1改成Func1，Func1可以為任意名字，並放在程式的開頭。
{% highlight c++ linenos %}
typedef int (*Func1)(int,string);
{% endhighlight %}

修改完的程式碼如下
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
//宣告Func1類型別名
typedef int (*Func1)(int,string);
int func1(int code, string msg) {
  cout << "Err code = " << code << ", msg = " << msg << endl;
  return code;
}
int main() {
  //宣告指標變數pf1為Func1類型
  Func1 pf1; //宣告函式指標變數pf1
  pf1 = func1; //函式指標變數pf1設定函式
  pf1(404, "Page not found.");//使用函式指標pf1呼叫函式
  return 0;
}
{% endhighlight %}

```
執行結果
Err code = 404, msg = Page not found.
```

### 函式參數是函式指標

宣告函式print404Msg()，第一個參數為函式指標，函式的資料型態是傳回值為int，函式指標名為pf1，2個參數資料型態分別為int跟string。

{% highlight c++ linenos %}
void print404Msg(int (*pf1)(int,string), string msg) {
  pf1(404, msg);
}
{% endhighlight %}

第1行,宣告函式print404Msg()，第一個參數為函式指標，第二個參數為string資料型態。

第2行,使用函式指標pf1呼叫函式，並把404整數與msg的參數傳進函式。

完整程式
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void print404Msg(int (*pf1)(int,string), string msg) {
  pf1(404, msg);
}
int func1(int code, string msg) {
  cout << "Err code = " << code << ", msg = " << msg << endl;
  return code;
}
int main() {
  print404Msg(func1, "Page not Found");
  return 0;
}
{% endhighlight %}

```
執行結果
Err code = 404, msg = Page not Found
```

### 函式參數是typedef函式指標類型別名

也可以使用typedef函式指標類型另取別名。

{% highlight c++ linenos %}
//宣告Func1類型別名
typedef int (*Func1)(int,string);
//第一個參數資料型態為Func1
void print404Msg(Func1 pf1, string msg) {
  pf1(404, msg);
}
{% endhighlight %}


完整程式
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
typedef int (*Func1)(int,string);
void print404Msg(Func1 pf1, string msg) {
  pf1(404, msg);
}
int func1(int code, string msg) {
  cout << "Err code = " << code << ", msg = " << msg << endl;
  return code;
}
int main() {
  print404Msg(func1, "Page not Found");
  return 0;
}
{% endhighlight %}

### 函式指標應用

自訂二個函式指標類型別名
{% highlight c++ linenos %}
//宣告類型別名
//傳回值為void，別名為Success，參數資料型態char*指標
typedef void (*Success)(char*);
//傳回值為void，別名為Failure，參數類型分別為int，char*指標
typedef void (*Failure)(int, char*);
{% endhighlight %}


{% highlight c++ linenos %}
void httpOk(char* msg) {
  printf("成功，結果:%s\n", msg);
}
void httpFailure(int code, char* msg) {
  printf("失敗%d，原因:%s\n", code, msg);
}
{% endhighlight %}
第1行,宣告函式，傳回值與參數資料型態都符合函式指標類型別名Success

第4行,宣告函式，傳回值與參數資料型態都符合函式指標類型別名Failure

{% highlight c++ linenos %}
void http(int res, Success success, Failure failure) {
  if(res == 1) {
    success("取得資料成功");
  } else {
    failure(505,"網路連線有問題");
  }
}
int main() {
  http(1,httpOk,httpFailure);
  http(0,httpOk,httpFailure);
  return 0;
}
{% endhighlight %}
第1行,宣告函式，第1個參數資料型態int，第2個參數函式指標類型別名Success，第3個參數函式指標類型別名Failure。

第3行,使用函式指標Success呼叫函式。

第5行,使用函式指標Failure呼叫函式，並傳入參數。

第9行,呼叫http函式，並把函式傳回值參數與自訂Success函式資料型態一樣的httpOk函式傳入。

第10行,呼叫http函式，並把函式傳回值參數與自訂Failure函式資料型態一樣的httpFailure函式傳入。

完整程式

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
//函式指標類型別名
typedef void (*Success)(char*);
typedef void (*Failure)(int, char*);
void httpOk(char* msg) {
  printf("成功，結果:%s\n", msg);
}
void httpFailure(int code, char* msg) {
  printf("失敗%d，原因:%s\n", code, msg);
}
void http(int res, Success success, Failure failure) {
  if(res == 1) {
    success("取得資料成功");
  } else {
    failure(505,"網路連線有問題");
  }
}
int main() {
  http(1,httpOk,httpFailure);
  http(0,httpOk,httpFailure);
  return 0;
}
{% endhighlight %}

```
執行結果
成功，結果:取得資料成功
失敗505，原因:網路連線有問題
```

[1]: {% link _pages/c/basic/typedef.md %}