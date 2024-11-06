---
title: forward
date: 2024-11-6
keywords: c++, forward
---

Prerequisites:
- [l-value與r-value][1]
- [l-value參考與r-value參考][2]

## 參數為左值與右值

呼叫的函式的參數為左值與右值，相同的函式名，但參數多載(overload)。

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
//參數左值
void func(int& x) {
    cout << "l-value" << endl;
}
//參數右值
void func(int&& x) {
    cout << "r-value" << endl;
}
int main() {
    int i = 10;
    //呼叫函式，參數為左值
    func(i);
    //呼叫函式，參數為右值
    func(100);
    return 0;
}
{% endhighlight %}
```
l-value
r-value
```

## 保留右值參考的屬性

以下的函式，多出了middle()的函式，目的是再一次傳送參數，執行的結果會導致全被視作左值。

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
//參數左值
void func(int& x) {
    cout << "l-value" << endl;
}
//參數右值
void func(int&& x) {
    cout << "r-value" << endl;
}
void middle(int x) {
    func(x);
}
int main() {
    int i = 10;
    //呼叫函式，參數為左值
    middle(i);
    //呼叫函式，參數為右值
    middle(100);
    return 0;
}
{% endhighlight %}
```
l-value
l-value
```

為了解決以上的問題，需寫一個template，可以接收右值與左值，並把左右值屬性保留住，傳到func()的函式

### T&&

若模板的類型為T&&，代表可以接收左值與右值，注意！這邊的T&&不是只能接收右值，也可以不接收左值，跟一般函式參數int&&是不一樣，函式參數類型為int&&就能接收右值。

### forward<T>

左右值屬性保留住，傳到其它函式。

### 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
//參數左值
void func(int& x) {
    cout << "l-value" << endl;
}
//參數右值
void func(int&& x) {
    cout << "r-value" << endl;
}
template<typename T>
void middle(T&& x) {
    func(forward<T>(x));
}
int main() {
    int i = 10;
    //呼叫函式，參數為左值
    middle(i);
    //呼叫函式，參數為右值
    middle(100);
    return 0;
}
{% endhighlight %}
```
l-value
r-value
```

[1]: {% link _pages/c/c11/rvalue/l_r_value.md %}
[2]: {% link _pages/c/c11/rvalue/l_r_ref.md %}