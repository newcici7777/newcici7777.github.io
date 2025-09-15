---
title: C++中使用C
date: 2025-09-12
keywords: c++, c,
---
定義一個c的頭文件

{% highlight c++ linenos %}
//定義一個c函式
void test(int x, int y);
{% endhighlight %}

實作頭文件

{% highlight c++ linenos %}
#include <stdio.h>
#include "a.h"

//實作頭文件定義的c函式
void test(int x, int y){
    printf("x=%d, y=%d\n", x, y);
}
{% endhighlight %}

C++檔案中使用C

在頭文件引入的部分要用<mark style="color:red;">extern "C"{}</mark>包起來，因為這是C的程式

{% highlight c++ linenos %}
#include <iostream>
extern "C"{
#include "a.h"
}
int main() {
    test(1,2);
}
{% endhighlight %}

執行結果

```
x=1, y=2
```

也可以直接由頭文件判斷是否是在c++的環境中，透過\_\_cplusplus的宏定義，若是在c++的環境，就要加上extern "C"{}

{% highlight c++ linenos %}
#ifdef __cplusplus
extern "C" {
#endif
//定義一個c函式
void test(int x, int y);
#ifdef __cplusplus
}
#endif

{% endhighlight %}

在main.cpp就不用再判斷extern "C"{}

{% highlight c++ linenos %}
#include <iostream>
#include "a.h"
int main() {
    test(1,2);
}
{% endhighlight %}

