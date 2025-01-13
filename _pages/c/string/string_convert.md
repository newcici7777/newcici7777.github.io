---
title: string convert
date: 2024-12-11
keywords: c++, string
---

Prerequisites:

- [string][1]


## to_string

將整數轉成string
{% highlight c++ linenos %}
  string str = to_string(2022 + 23);
  cout << "str = " << str << endl;
{% endhighlight %}
```
str = 2045
```

## string to char指標

{% highlight c++ linenos %}
  // char指標陣列
#include <iostream>
using namespace std;
int main() {
  char* arr[10];
  arr[0] = (char*)"test";
  arr[1] = (char*)"/123";
  cout << "arr[0] = " << arr[0] << endl;
  cout << "arr[1] = " << arr[1] << endl;
  return 0;
}
{% endhighlight %}
```
arr[0] = test
arr[1] = /123
```
## const string to char指標

- [strcpy][2]
- [c_str][3]

c_str()是取得string物件位址

{% highlight c++ linenos %}
  const string title = "被討厭的勇氣";
  char* s = new char[100];
  strcpy(s, title.c_str());
  cout << "s = " << s << endl;
{% endhighlight %}
```
s = 被討厭的勇氣
```

## char* to string
- [字串連接][4]

{% highlight c++ linenos %}
#include <iostream>
#include <cstring>
using namespace std;
int main() {
  // 有const char*
  const char* c_s1 = "Hello world!";
  // 沒const char*
  char* c_s2 = " Hi! Mary!";
  // 把其中一個char*變成string，就可以套用operator+= (const char* s)
  string s1 = string(c_s1) + c_s2;
  cout << "s1 = " << s1 << endl;
  return 0;
}
{% endhighlight %}
```
s1 = Hello world! Hi! Mary!
```

[1]: {% link _pages/c/stl/string.md %}
[2]: {% link _pages/c/array/charArray.md %}#字串指標拷貝-strcpy
[3]: {% link _pages/c/stl/string.md %}#取得string物件存放字串的記憶體位址
[4]: {% link _pages/c/stl/string.md %}#字串連接