---
title: pair
date: 2024-12-06
keywords: c++, pair
---

Prerequisites:

- [函式模板][1]
- [initializer list][3]

## pair模板與make pair模板

### pair模板
{% highlight c++ linenos %}
template <class T1, class T2>
struct pair 
{ 
  T1 first;   // key
  T2 second;  // value。
  pair();   // 建構子
  pair(const T1 &val1,const T2 &val2);   // 有二個參數建構子
  pair(const pair<T1,T2> &p);   // 拷貝函式
  void swap(pair<T1,T2> &p);   // 交換pair
};
{% endhighlight %}

### make pair模板

- [匿名物件][2]

呼叫pair的二個參數建構子，傳回pair的匿名物件

{% highlight c++ linenos %}
template <class T1, class T2>
make_pair(const T1 &first,const T2 &second)
{
  return pair<T1,T2>(first, second);
}
{% endhighlight %}

## 使用pair

## 呼叫2個參數的建構子
呼叫2個參數的建構子
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  pair<int,string> p1(1,"cici");
  cout << "p1 first = " << p1.first << ", second = " << p1.second << endl;
  return 0;
}
{% endhighlight %}
```
p1 first = 1, second = cici
```

## 使用initializer list

\{ 2, "Bill" \} 為匿名物件。

{% highlight c++ linenos %}
  pair<int,string> p2 = {2, "Bill"};
  cout << "p2 first = " << p2.first << ", second = " << p2.second << endl;
{% endhighlight %}
```
p2 first = 2, second = Bill
```

## 呼叫拷貝函式

- 先建立匿名物件，呼叫2個參數建構子
- 再用拷貝函式，把匿名物件設給p3

{% highlight c++ linenos %}
  pair<int,string> p3 = pair<int,string>(3, "Jeff");
  cout << "p3 first = " << p3.first << ", second = " << p3.second << endl;
{% endhighlight %}
```
p3 first = 3, second = Jeff
```

## makepair

以下是c++14運行的結果
- 先建立匿名物件，呼叫2個參數建構子
- 再用拷貝函式，把匿名物件拷貝到回傳值
- 再用拷貝函式，把回傳值拷貝到p4

{% highlight c++ linenos %}
  pair<int,string> p4 = make_pair<int,string>(4, "Ann");
  cout << "p4 first = " << p4.first << ", second = " << p4.second << endl;
{% endhighlight %}
```
p4 first = 4, second = Ann
```




[1]: {% link _pages/c/template/function_template.md %}
[2]: {% link _pages/c/class/anonymous_obj.md %}
[3]: {% link _pages/c/c11/init_list.md %}