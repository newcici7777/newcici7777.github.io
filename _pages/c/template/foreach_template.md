---
title: foreach模板
date: 2024-11-29
keywords: c++, template, foreach
---

Prerequisites:

- [iterator][1]

## 傳統思維

試想一下，寫一個foreach函式，傳統思維會將容器(vector,list)位址傳入函式，如以下的寫法:

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
void foreach(const vector<int>& v) {
  for (auto it = v.cbegin(); it != v.end(); it++) {
  cout << *it << " ";
  }
  cout << endl;
}
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  foreach(v1);
  return 0;
}
{% endhighlight %}
```
1 2 3 4 5 6 7 8 9 10
```

## 疊代器思維

STL容器(vector,list)皆有iterator疊代器，把iterator疊代器作為參數代入函式，有以上相同效果。
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
void foreach(const vector<int>::iterator first, const vector<int>::iterator last) {
  for (auto it = first; it != last; it++) {
  cout << *it << " ";
  }
  cout << endl;
}
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  foreach(v1.begin(), v1.end());
  return 0;
}
{% endhighlight %}
```
1 2 3 4 5 6 7 8 9 10 
```

## 函式多載(overload)

若容器元素的類型為string，就要多寫一個支援容器元素為string的函式。

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
void foreach(const vector<int>::iterator first, const vector<int>::iterator last) {
  for (auto it = first; it != last; it++) {
  cout << *it << " ";
  }
  cout << endl;
}
void foreach(const vector<string>::iterator first, const vector<string>::iterator last) {
  for (auto it = first; it != last; it++) {
  cout << *it << " ";
  }
  cout << endl;
}
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  foreach(v1.begin(), v1.end());
  vector<string> v2 = {"首頁", "客服", "購物車", "個人資訊"};
  foreach(v2.begin(), v2.end());
  return 0;
}
{% endhighlight %}
```
1 2 3 4 5 6 7 8 9 10 
首頁 客服 購物車 個人資訊
```

## 模板取代函式多載

類型全換成T，不管T是什麼類型，只要支持函式中的assign(=)、!=、it++、*it的功能就可以使用。

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
template<typename T>
void foreach(const T first, const T last) {
  for (auto it = first; it != last; it++) {
  cout << *it << " ";
  }
  cout << endl;
}
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  foreach(v1.begin(), v1.end());
  vector<string> v2 = {"首頁", "客服", "購物車", "個人資訊"};
  foreach(v2.begin(), v2.end());
  return 0;
}
{% endhighlight %}
```
1 2 3 4 5 6 7 8 9 10 
首頁 客服 購物車 個人資訊
```

## callback模板函式(回調/回呼函式)

在模板函式中要做的事由callback函式去完成。

### callback函式為函式指標

寫一個callback函式支援容器元素為string，並印出元素的值。
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
void callback(const string& msg) {
  cout << msg << " ";
}
template<typename T>
void foreach(const T first, const T last, void(*callback)(const string&)) {
  for (auto it = first; it != last; it++) {
  callback(*it);
  }
  cout << endl;
}
int main() {
  vector<string> v2 = {"01", "02", "03", "04", "05"};
  foreach(v2.begin(), v2.end(), callback);
  return 0;
}
{% endhighlight %}
```
01 02 03 04 05 
```

### callback函式為物件函式

- [物件函式][2]
- [匿名物件][3]

呼叫函式物件，使用匿名物件方式呼叫

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
class Callback {
 public:
  void operator()(const string& msg) {
  cout << msg << " ";
  }
};
template<typename T>
void foreach(const T first, const T last, Callback callback) {
  for (auto it = first; it != last; it++) {
  callback(*it);
  }
  cout << endl;
}
int main() {
  vector<string> v2 = {"01", "02", "03", "04", "05"};
  foreach(v2.begin(), v2.end(), Callback());  // 呼叫函式物件，使用匿名物件方式呼叫
  return 0;
}
{% endhighlight %}
```
01 02 03 04 05
```

### 模板取代函式指標與物件函式

從以上二個程式碼，可以發現呼叫函式指標與物件函式的方式都一樣，因此把foreach函式中的Callback參數化為模板，參數可以為函式指標或物件函式。
```
callback(*it);
```

把callback類型化為模板T2

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
void callback(const string& msg) {
  cout << msg << " ";
}
class Callback {
 public:
  void operator()(const string& msg) {
  cout << msg << " ";
  }
};
//把callback類型化為模板T2
template<typename T1, typename T2>
void foreach(const T1 first, const T1 last, T2 callback) {
  for (auto it = first; it != last; it++) {
  callback(*it);
  }
  cout << endl;
}
int main() {
  vector<string> v2 = {"01", "02", "03", "04", "05"};
  //物件函式
  foreach(v2.begin(), v2.end(), Callback());
  //函式指標
  foreach(v2.begin(), v2.end(), callback);
  return 0;
}
{% endhighlight %}

### callback函式參數模板化

以上函式的缺點在於函式參數固定是string&的類型。

將callback函式與類別的參數模板化

#### 函式模板
{% highlight c++ linenos %}
template<typename T>
void callback(const T& msg) {
  cout << msg << " ";
}
{% endhighlight %}

#### 類別模板
{% highlight c++ linenos %}
template<typename T>
class Callback {
 public:
  void operator()(const T& msg) {
  cout << msg << " ";
  }
};
{% endhighlight %}

#### 呼叫函式模板、類別模板

呼叫模板函式與模板類別，記得加上\<型別\>

{% highlight c++ linenos %}
  //物件函式
  foreach(v2.begin(), v2.end(), Callback<string>());
  //函式指標
  foreach(v2.begin(), v2.end(), callback<string>);
{% endhighlight %}

#### 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
template<typename T>
void callback(const T& msg) {
  cout << msg << " ";
}
template<typename T>
class Callback {
 public:
  void operator()(const T& msg) {
  cout << msg << " ";
  }
};
template<typename T1, typename T2>
void foreach(const T1 first, const T1 last, T2 callback) {
  for (auto it = first; it != last; it++) {
  callback(*it);
  }
  cout << endl;
}
int main() {
  vector<string> v2 = {"01", "02", "03", "04", "05"};
  //物件函式
  foreach(v2.begin(), v2.end(), Callback<string>());
  //函式指標
  foreach(v2.begin(), v2.end(), callback<string>);
  return 0;
}
{% endhighlight %}
```
01 02 03 04 05 
01 02 03 04 05 
```

## 使用官方for_each

以上已實作官方for_each模板函式，以下為使用官方for_each

### include

{% highlight c++ linenos %}
#include <algorithm>
{% endhighlight %}

### for_each
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
template<typename T>
void callback(const T& msg) {
  cout << msg << " ";
}
template<typename T>
class Callback {
 public:
  void operator()(const T& msg) {
  cout << msg << " ";
  }
};
int main() {
  vector<string> v2 = {"01", "02", "03", "04", "05"};
  //物件函式
  for_each(v2.begin(), v2.end(), Callback<string>());
  //函式指標
  for_each(v2.begin(), v2.end(), callback<string>);
  return 0;
}
{% endhighlight %}
```
01 02 03 04 05 01 02 03 04 05
```


[1]: {% link _pages/c/stl/iterator.md %}
[2]: {% link _pages/c/class/functor.md %}
[3]: {% link _pages/c/class/anonymous_obj.md %}