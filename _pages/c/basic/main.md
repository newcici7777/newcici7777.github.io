---
title: main
date: 2025-01-13
keywords: c++, main
---
Prerequisites:
- [linux下編譯c++][1]

## main函式
main()函式為程式進入點，若沒有要抓參數，可以不寫main的參數，預設參數為空，函式傳回值預設傳0。

### main函式的參數

{% highlight c++ linenos %}
int main(int argc, char* argv[])
{% endhighlight %}
- argc 參數個數
- argv[] 參數的值

### 程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main(int argc, char* argv[]) {
  cout << "參數個數 = " << argc << endl;
  // 全部參數
  for (int i = 0; i < argc; i++) {
    cout << "第" << i << "個參數 = " << argv[i] << endl;
  }
  return 0;
}
{% endhighlight %}

### 編譯執行
```
$ vi main_test.cpp
$ g++ -o main_test main_test.cpp
$ ./main_test
參數個數 = 1
第0個參數 = ./main_test
```
由以上結果可知，`./main_test`，也是一個參數

```
$ /home/cici/test/app/main_test
參數個數 = 1
第0個參數 = /home/cici/test/app/main_test
```
由以上結果可知，輸入執行檔的全部路徑，全部路徑也是一個參數

```
$ ./main_test aa bb cc dd ee ff
參數個數 = 7
第0個參數 = ./main_test
第1個參數 = aa
第2個參數 = bb
第3個參數 = cc
第4個參數 = dd
第5個參數 = ee
第6個參數 = ff
```

[1]: {% link _pages/c/compile/makefile.md %}