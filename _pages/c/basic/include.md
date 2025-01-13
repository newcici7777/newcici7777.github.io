---
title: include與define
date: 2025-01-13
keywords: c++, include, define
---

## head file

檔名以.h為結尾，定義使用的標頭檔，定義(definition)函式，定義類別，定義結構...等等。

## include head file
從編譯器中的Library找尋標頭檔:
{% highlight c++ linenos %}
#include <標頭檔>
{% endhighlight %}

檔案所在目錄下，尋找副檔名.h標頭檔，找不到就去編譯器中的Library找尋。
{% highlight c++ linenos %}
#include "標頭檔.h"
{% endhighlight %}

include也可以用.cpp檔，不是只限於.h

include是把檔案內容複製下來，貼上呼叫include的位置，跟[內嵌函式][1]是一樣的。

## pragma once
重覆include的標頭檔，只要include一次，放在檔案最上面。
{% highlight c++ linenos %}
#pragma once
{% endhighlight %}

## C Standard Library
c98的Standard Library，舊版本會加上.h，新版本前面會加上c去掉.h

不加上std的namespace也可以使用C Standard Library

舊版本
{% highlight c++ linenos %}
#include <stdio.h>
{% endhighlight %}

新版本前面會加上c去掉.h
{% highlight c++ linenos %}
#include <cstdio>
{% endhighlight %}

## C++ Standard Library
要加上std的namespace才可以使用C++ Standard Library
{% highlight c++ linenos %}
using namespace std;
{% endhighlight %}

舊版本會加上.h，新版本會去掉.h，已棄用。
{% highlight c++ linenos %}
#include <iostream.h>
{% endhighlight %}

新版本會去掉.h
{% highlight c++ linenos %}
#include <iostream>
{% endhighlight %}

## define前置指令
編譯的時候，編譯器把以下的內容取代變數，跟[內嵌函式][1]是一樣的。

define只是把內容取代變數，沒有變數型別。

{% highlight c++ linenos %}
#define 變數 內容
{% endhighlight %}

以下將"ABCDEFaaaaaa"取代MSG
{% highlight c++ linenos %}
#define MSG "ABCDEFaaaaaa"
int main() {
  cout << MSG << endl;
  return 0;
}
{% endhighlight %}
```
ABCDEFaaaaaa
```

把MSG變數的內容改成`endl;`斷行與分號，以下程式碼編譯不會出錯。
{% highlight c++ linenos %}
#define MSG endl;
int main() {
  // 注意，以下結尾沒有分號
  cout << MSG
  return 0;
}
{% endhighlight %}
```

```
結果只會出現一個斷行

### C++提供的前置指令

`__cplusplus` ，可以辯別是c++還c

檔名 : `__FILE__`

函式名 : `__Function__`

程式碼行號 : `__LINE__`

編譯日期: `__DATE__`

編譯時間: `__TIME__`

{% highlight c++ linenos %}
int main() {
  cout << "__cplusplus = " << __cplusplus << endl;
  cout << "__FILE__ = " << __FILE__ << endl;
  cout << "__FUNCTION__ = " << __FUNCTION__ << endl;
  cout << "__LINE__ = " << __LINE__ << endl;
  cout << "__DATE__ = " << __DATE__ << endl;
  cout << "__TIME__ = " << __TIME__ << endl;
  cout << "__TIMESTAMP__ = " << __TIMESTAMP__ << endl;
  return 0;
}
{% endhighlight %}
```
__cplusplus = 201402
__FILE__ = /Users/cici/projects/lsn11/lsn11/main.cpp
__FUNCTION__ = main
__LINE__ = 20
__DATE__ = Jan 13 2025
__TIME__ = 13:41:38
__TIMESTAMP__ = Mon Jan 13 13:41:36 2025
```

### define 只有變數沒有內容
定義前置變數，並不是一定要有內容
{% highlight c++ linenos %}
#define 變數
{% endhighlight %}

例:
{% highlight c++ linenos %}
#define DEBUG
{% endhighlight %}

### ifdef
判斷是否有定義前置變數
{% highlight c++ linenos %}
#define DEBUG
int main() {
#ifdef DEBUG // 若有定義DEBUG
  printf("測試測試\n");
#else  // 若沒有定義DEBUG
  printf("不是測試\n");
#endif
  return 0;
}
{% endhighlight %}
```
測試測試
```
### ifndef
判斷若沒定義

以下的程式碼功能跟pragma once一樣，只會被include一次
{% highlight c++ linenos %}
using namespace std;
#ifndef __CICI__H  // 若沒有定義CICI__H
#define __CICI__H  // 定義它
#include "cici.h"  // 匯入cici.h的內容
#endif
int main() {
  hi();
  return 0;
}
{% endhighlight %}
```
Hello! Cici!
```

cici.h的內容如下:
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void hi() {
  cout << "Hello! Cici!" << endl;
}
{% endhighlight %}

### 判斷作業系統
Linux : `__linux__`

Windows : `_WIN32`

{% highlight c++ linenos %}
int main() {
#ifdef _WIN32  // 判斷是不是windows
  cout << "這是windows" << endl;
#else
  cout << "這不是windows" << endl;
#endif
  return 0;
}
{% endhighlight %}
```
這不是windows
```

### 不同作業系統，定義型別

在windows中，long是4byte(32bit)，long long是8byte(64bit)

在linux中，long是8byte(64bit)，二者的long的bit不同。

以下範例自行定義64bit的整數型別，int64，根據判斷作業系統，採用不同的long，但型別的儲存大小都是8byte(64bit)。
{% highlight c++ linenos %}
int main() {
#ifdef _WIN32 //判斷是不是windows
  cout << "這是windows" << endl;
  typedef long long int64;
#else
  cout << "這不是windows" << endl;
  typedef long int64;
#endif
  int64 val = 100;
  cout << "val = " << val << endl;
  return 0;
{% endhighlight %}
```
這不是windows
val = 100
```

[1]: {% link _pages/c/function/func_inline.md %}
