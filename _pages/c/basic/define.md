---
title: define
date: 2025-07-27
keywords: c++, c, define
---
define是在編譯「前」就進行前置處理，沒辦法debug。

```mermaid
flowchart LR
    程式碼 -- 前置處理 --> 編譯
```

## define 宣告常數
在main()主程式以前，設定常數名與值，後面(尾部)沒有分號`;`。<br>
語法
```
#define 常數名 值
int main() {
  return 0;
}
```

{% highlight c++ linenos %}
#define PI 3.14
int main() {
  cout << PI << endl; 
  return 0;
}
{% endhighlight %}

## 無法修改define
以下程式碼會編譯錯誤，常數不可再被修改。
{% highlight c++ linenos %}
#define PI 3.14
int main() {
  PI = 2.55;  // 編譯錯誤
  cout << PI << endl;
  return 0;
}
{% endhighlight %}

## define取代
編譯的時候，編譯器把以下的值取代常數名，跟[內嵌函式][1]是一樣的。<br>
define只是把值取代常數名。<br>

{% highlight c++ linenos %}
#define 常數名 值
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

把MSG常數的值改成`endl;`斷行與分號，以下程式碼編譯不會出錯。
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

## define取代與運算
以下的運算結果為10，define是一個取代的過程。
1. B 變成 1 + 3
2. C 變成 1 / 1 + 3 * 3
{% highlight c++ linenos %}
#define A 1
#define B A + 3
#define C A / B * 3
int main() {
  printf("%d\n", C);
  return 0;
}
{% endhighlight %}
```
10
```

### 加上括號
取代過程如下:
1. B 變成 (1 + 3)
2. C 變成 1 / (1 + 3) * 3
3. C 變成 1 / 4 * 3
{% highlight c++ linenos %}
#define A 1
#define B (A + 3)
#define C A / B * 3
int main() {
  printf("%.2f\n", C);
  return 0;
}
{% endhighlight %}
```
0.0
```

### 整數與浮點數
但為何以上的結果是0.0呢？<br>
原因是1 / 4是整數相除，整數會無條件去掉小數點以後的數字，就會變成0。<br>
改成1.0 / 4，就會把整個運算式變成double運算，小數點就會被留下來。<br>
{% highlight c++ linenos %}
#define A 1.0
#define B (A + 3)
#define C A / B * 3
int main() {
  printf("%.2f\n", C);
  return 0;
}
{% endhighlight %}
```
0.75
```

## undef清除常數
undef可以清除已宣告的常數。
{% highlight c++ linenos %}
#define PI 3.14159
#undef PI
{% endhighlight %}

## define 只有常數名沒有值
定義常數名，並不是一定要有值。<br>
{% highlight c++ linenos %}
#define 常數名
{% endhighlight %}

例:
{% highlight c++ linenos %}
#define DEBUG
{% endhighlight %}

### ifdef
判斷是否有定義常數名。<br>
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

加上undef清除常數。
{% highlight c++ linenos %}
#define DEBUG
#undef DEBUG
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
不是測試
```

如果沒使用undef在沒有值的常數名，即便下一次編譯時，仍會殘存上一次DEBUG的常數名，就不會執行else的部分。(也有可能是我使用xcode編譯器的問題。)
{% highlight c++ linenos %}
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
若沒有define
{% highlight c++ linenos %}
#define DEBUG
#undef DEBUG
int main() {
#ifndef DEBUG
  printf("不是測試\n");
#endif
  return 0;
}
{% endhighlight %}
```
不是測試
```

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