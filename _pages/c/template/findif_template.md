---
title: find_if模板
date: 2024-11-29
keywords: c++, template, find_if
---

- [continue][1]
- [物件函式][2]
- [foreach模板][3]

find_if()函式

若在容器中找到元素就返回疊代器iterator，找不到就返回容器最後一個元素的下一個位址

## 增加搜尋值與傳回值

將foreach模板的程式拿來修改，增加搜尋值T3的模板參數與T1的模板回傳值。

找到欲搜尋的值T3，就返回iterator

找不到就回傳容器最後一個元素的下一個位址。

{% highlight c++ linenos %}
template<typename T1, typename T2, typename T3>
T1 findif(const T1 first, const T1 last, T2 callback, T3 search_val) {
  for (auto it = first; it != last; it++) {
    if(callback(*it, search_val) == true) return it;
  }
  return last;
}
{% endhighlight %}

## 修改callback函式指標模板

增加搜尋值T的模板參數，回傳bool，判斷msg是否與要搜尋的值相同。

{% highlight c++ linenos %}
template<typename T>
bool callback(const T& msg, const T& search_val) {
  return msg == search_val;
}
{% endhighlight %}

## 修改callback物件函式模板

增加搜尋值T的模板參數，回傳bool，判斷msg是否與要搜尋的值相同。

{% highlight c++ linenos %}
template<typename T>
class Callback {
 public:
  bool operator()(const T& msg, const T& search_val) {
    return msg == search_val;
  }
};
{% endhighlight %}

## 呼叫findif模板

傳回值為iterator，把iterator指標的值印出來。
{% highlight c++ linenos %}
  vector<string>::iterator it1 = findif(v2.begin(), v2.end(), Callback<string>(), "03");
  cout << "找到 = " << *it1 << endl;
{% endhighlight %}


## 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
template<typename T>
bool callback(const T& msg, const T& search_val) {
  return msg == search_val;
}
template<typename T>
class Callback {
 public:
  bool operator()(const T& msg, const T& search_val) {
    return msg == search_val;
  }
};
template<typename T1, typename T2, typename T3>
T1 findif(const T1 first, const T1 last, T2 callback, T3 search_val) {
  for (auto it = first; it != last; it++) {
    if(callback(*it, search_val) == true) return it;
  }
  return last;
}
int main() {
  vector<string> v2 = {"01", "02", "03", "04", "05"};
  //物件函式
  vector<string>::iterator it1 = findif(v2.begin(), v2.end(), Callback<string>(), "03");
  cout << "找到 = " << *it1 << endl;
  //函式指標
  vector<string>::iterator it2 = findif(v2.begin(), v2.end(), callback<string>, "05");
  cout << "找到 = " << *it2 << endl;
  return 0;
}
{% endhighlight %}
```
找到 = 03
找到 = 05
```

## 物件函式，使用建構子，搜尋值作為參數

- [建構子初始化][4]
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
template<typename T>
class Callback {
 public:
  T search_val;
  Callback(const T& search_val):search_val(search_val) {}
  bool operator()(const T& msg) {
    return msg == search_val;
  }
};
template<typename T1, typename T2>
T1 findif(const T1 first, const T1 last, T2 callback) {
  for (auto it = first; it != last; it++) {
    if(callback(*it) == true) return it;
  }
  return last;
}
int main() {
  vector<string> v2 = {"01", "02", "03", "04", "05"};
  //物件函式
  vector<string>::iterator it1 = findif(v2.begin(), v2.end(), Callback<string>("03"));
  cout << "找到 = " << *it1 << endl;
  return 0;
}
{% endhighlight %}


技巧:

在迴圈中，若函式()返回true，就continue回到迴圈的條件判斷式，返回false就繼續執行接下來的程式碼。

之後sort模板會利用到這個技巧。




[1]: {% link _pages/c/basic/loop.md %}#continue-回到迴圈的條件判斷式
[2]: {% link _pages/c/class/functor.md %}
[3]: {% link _pages/c/template/foreach_template.md %}
[4]: {% link _pages/c/class/init_list.md %}
