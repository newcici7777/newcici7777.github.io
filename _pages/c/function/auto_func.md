---
title: auto與函式
date: 2024-10-29
keywords: c++, auto
---

Prerequisites:

[函式指標][1]

auto的中文意思為自動推導類型，使用時不用特別寫準確的類型，編譯器會自動推導。

## 自動推導函式傳回值類型

注意！這邊的函式名後面有括號()，auto是函式傳回值類型。

語法
```
auto 變數 = 函式名();
auto var = func();
```
{% highlight c++ linenos %}
//宣告一個函式為func()，傳回值為string類型
string func() {
  return "test";
}
int main() {
	//使用函式的傳回值，由auto自動推導函式的傳回的類型string
  auto var = func();
  cout << "var = " << var << endl;
  return 0;
}
{% endhighlight %}

```
test
```

## 自動推導函式指標類型

函數指針設計複雜，可以使用auto簡化。

注意！這邊的函式名後面沒有括號，auto指的是函式指標類型。

語法
```
auto 函式指標名 = 函式名;
auto f = func;

呼叫函式
f();
```

完整程式碼
{% highlight c++ linenos %}
string func() {
  cout << "abcdef" << endl;
  return "test";
}
int main() {
	//注意!函式func後面沒有括號
  auto f = func;
  //呼叫函式
  cout << f() << endl;
  return 0;
}
{% endhighlight %}

```
abcdef
```

## 簡化函式指標

參數很多的函式，可用auto簡化函式指標。

### 未簡化前

{% highlight c++ linenos %}
//定義一個函式名為println，傳回值為int，
//參數有二個，分別為char指標型別的參數msg1與msg2
int println(char* msg1, char* msg2) {
  printf("印出msg1:%s\nmsg2:%s\n",msg1,msg2);
  return 1;
}
int main() {
	//(*funcPtr)宣告函式指標的名稱為funcPtr
	//傳回值為int
	//(char*,char*)代表參數型別
	//實作函式指標的函式是println()的函式
	//注意= println後面是沒有括號()
  int (*funcPtr)(char*,char*)= println;

  //呼叫函式指標，並把參數代入
  funcPtr("test","c++");
  return 0;
}
{% endhighlight %}

```
印出msg1:test
msg2:c++
```

### 使用auto簡化函式指標

{% highlight c++ linenos %}
int println(char* msg1, char* msg2) {
  printf("印出msg1:%s\nmsg2:%s\n",msg1,msg2);
  return 1;
}
int main() {
  auto func = println;
  func("test","c++");
  return 0;
}
{% endhighlight %}

### 以下程式碼未簡化前

{% highlight c++ linenos %}
#include <iostream>
void say(void (*p)(char*),char *msg) {
  p(msg);
}
void println(char* msg) {
  printf("印出結果:%s\n",msg);
}
int main() {
  void(*p)(char*) = println;
  say(p, "hello");
  return 0;
}
{% endhighlight %}

```
印出結果:hello
```

### 使用auto簡化函式指標

{% highlight c++ linenos %}
//定義一個函式指標的類型為FuncPtr，傳回值為void的類型，參數是char指針
typedef void(*FuncPtr)(char*);

//參數為FuncPtr類型(傳回值為void的類型，參數是char指針)
void say(FuncPtr func1, char* msg) {
  //呼叫函式指標
  func1(msg);
}
void printMsg(char* msg) {
  cout << "msg:" << msg << endl;
}
int main() {
  auto say_fun = say;
  auto print_fun = printMsg;
  //注意print_fun後面沒有括號()
  say_fun(print_fun,"test");
  return 0;
}
{% endhighlight %}
```
msg:test
```

[1]: {% link _pages/c/function/functionPointer.md %}