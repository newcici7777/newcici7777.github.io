---
title: 可變參數模板
date: 2024-11-6
keywords: c++, variadic template
---

## 參數清單

以下程式碼"Bill","Mary","Tom","Allen"就是參數清單。
{% highlight c++ linenos %}
int main() {
    print("Bill","Mary","Tom","Allen");
    return 0;
}
{% endhighlight %}

## 遞迴呼叫

- T為類型
- ...Args為參數清單類型
- arg為每一次遞迴，就會從參數清單取出一個，並從清單移出
- args為每一次遞迴，未被取出的剩下參數清單

{% highlight c++ linenos %}
template<typename T, typename... Args>
void print(T arg, Args... args) {
    cout << "參數" << arg << endl;
    print(args...);
}
{% endhighlight %}

## 遞迴結束呼叫的函式

- 參數為空
- 傳回值型態void

{% highlight c++ linenos %}
void print() {
    cout << "end" << endl;
}
{% endhighlight %}

## 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
//print()傳回值void，參數為空()，待遞迴結束時會呼叫此函式
void print() {
    cout << "end" << endl;
}
//print是遞回函式template
//每呼叫一次就從args中拿出一個參數arg，其它的args又傳入print中遞迴呼叫
template<typename T, typename... Args>
void print(T arg, Args... args) {
    cout << "參數" << arg << endl;
    print(args...);
}
int main() {
    print("Bill","Mary","Tom","Allen");
    return 0;
}
{% endhighlight %}
```
參數Bill
參數Mary
參數Tom
參數Allen
end
```
## 其它函式呼叫遞迴模板，呼叫的函式也要變成模板

以下程式碼為了呼叫遞迴模板，所以本身函式也要變成模板，再把參數傳入遞迴模板

{% highlight c++ linenos %}
//定義參數的類型名稱Args
template<typename... Args>
void func(const string& school, Args... args) {
    cout << school << "的學生有:" << endl;
    print(args...);
}
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
//print()傳回值void，參數為空()，待遞迴結束時會呼叫此函式
void print() {
    cout << "end" << endl;
}
//print是遞迴函式template
//每呼叫一次就從args中拿出一個參數arg，其它的args又傳入print中遞迴呼叫
template<typename T, typename... Args>
void print(T arg, Args... args) {
    cout << arg << endl;
    print(args...);
}
template<typename... Args>
void func(const string& school, Args... args) {
    cout << school << "的學生有:" << endl;
    print(args...);
}
int main() {
	//只有第1個參數傳進func印出來
	//第1個參數之後都參數都是args，代入遞迴函式
    func("青草湖國小","Bill","Mary","Tom","Allen");
    return 0;
}
{% endhighlight %}
```
青草湖國小的學生有:
Bill
Mary
Tom
Allen
end
```