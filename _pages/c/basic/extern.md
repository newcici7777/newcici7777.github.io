---
title: extern
date: 2025-09-03
keywords: c++, extern
---
## extern外部全域變數
想使用其它.c或.cpp中的全域變數，使用extern。<br>

以下是test.cpp
{% highlight c++ linenos %}
#include <stdio.h>
int num = 100;
{% endhighlight %}

以下是main.cpp<br>
main.cpp要使用test.c的全域變數num，main.cpp要使用extern，把其它檔案的全域變數引入。<br>
```
extern 類型 其它檔案全域變數名;
```
{% highlight c++ linenos %}
extern int num;
int main() {
  cout << num << endl;
  return 0;
}
{% endhighlight %}
```
100
```

不能有同名的全域變數，使用外部全域變數，不能再定義同名的全域變數，以下編譯錯誤。<br>
{% highlight c++ linenos %}
extern int num;
int num = 20;
int main() {
  cout << num << endl;
  return 0;
}
{% endhighlight %}

## extern函式()
要使用其它檔案中的函式，除了include之外，也可以使用extern 函式()。<br>

以下是main2.cpp
{% highlight c++ linenos %}
#include <stdio.h>
void main2() {
  printf("call extern test() \n");
}
{% endhighlight %}

以下是main.cpp。<br>
```
extern 傳回類型 外部函式();
```
{% highlight c++ linenos %}
extern void main2();
int main() {
  main2();
  return 0;
}
{% endhighlight %}
```
call extern test() 
```

## extern 靜態變數
extern靜態變數跟extern全域變數的意思相反，意思是，其它檔案都不可以使用我的靜態變數。

以下是main2.cpp<br>
{% highlight c++ linenos %}
#include <stdio.h>
static int counter = 500;
{% endhighlight %}

以下是main.cpp，使用外部靜態變數執行會失敗。
{% highlight c++ linenos %}
extern int counter;
int main() {
  cout << counter << endl;
  return 0;
}
{% endhighlight %}

## extern 靜態函式
extern 靜態函式，代表其它檔案都不可以使用我的靜態函式。

以下是main2.cpp<br>
{% highlight c++ linenos %}
static int counter = 500;
static void main2() {
  printf("call extern test() \n");
}
{% endhighlight %}

以下是main.cpp<br>
{% highlight c++ linenos %}
extern void main2();
int main() {
  main2();
  return 0;
}
{% endhighlight %}