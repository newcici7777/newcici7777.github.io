---
title: l-value參考與r-value參考
date: 2024-06-24
keywords: c++, l-value reference, r-value reference
---
Prerequisites:

- [l-value與r-value][1]

## l-value參考

l-value參考指向變數

{% highlight c++ linenos %}
int main() {
    int i = 10;
    int& l_ref = i;
    cout << "l-value ref = " << l_ref << endl;
    cout << "=== modify ref === " << endl;
    l_ref = 55;
    cout << "l-value ref = " << l_ref << endl;
    return 0;
}
{% endhighlight %}

```
l-value ref = 10
=== modify ref === 
l-value ref = 55
```

## r-value參考

r-value參考指向數字

{% highlight c++ linenos %}
int main() {
    int&& r_ref = 100;
    cout << "r-value ref = " << r_ref << endl;
    cout << "=== modify ref === " << endl;
    r_ref = 1000;
    cout << "r-value ref = " << r_ref << endl;
    return 0;
}
{% endhighlight %}

```
r-value ref = 100
=== modify ref === 
r-value ref = 1000
```

## 函式參數為l-value參考
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int& l_ref) {
    l_ref = 20;
}
int main() {
    int i = 10;
    cout << "before i = " << i << endl;
    func(i);
    cout << "after i = " << i << endl;
    return 0;
}
{% endhighlight %}

```
before i = 10
after i = 20
```
## 函式參數為r-value參考

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int&& r_ref) {
    cout << "r-value = " << r_ref << endl;
}
int main() {
    func(1000);
    return 0;
}
{% endhighlight %}

```
r-value = 1000
```

## 引數與參數r-value參考資料型態不同

[引數][2]與參數的資料型態不同，若自動轉型可以轉成功，也可以使用r-value參考。

{% highlight c++ linenos %}
void func1(int&& r_ref) {
    cout << "r-value = " << r_ref << endl;
}
int main() {
    func1('B');
    return 0;
}
{% endhighlight %}

```
r-value = 66
```

[1]: {% link _pages/c/reference/l_r_value.md %}
[2]: {% link _pages/c/pointer/pointerParam.md %}#引數-argument