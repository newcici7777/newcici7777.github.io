---
title: 可變參數模板
date: 2024-11-6
keywords: c++, variadic template
---
## 語法

Args代表多個不同類型與數量的參數。

以下三種...位置不同，都可以。

```
template<typename... Args>
```

```
template<typename ...Args> 
```

```
template<typename ... Args>
```

## 參數清單

以下程式碼"Bill","Mary","Tom","Allen"就是參數清單。

{% highlight c++ linenos %}
int main() {
    print("Bill","Mary","Tom","Allen");
    return 0;
}
{% endhighlight %}

## 遞迴呼叫

以下的模板是遞迴的方式在運作

- T為類型
- ...Args為參數清單類型
- arg為每一次遞迴，就會從參數清單取出一個
- args為每一次遞迴，未被取出的剩下參數清單

{% highlight c++ linenos %}
template<typename T, typename... Args>
void print(T arg, Args... args) {
	//每呼叫一次就從args中拿出一個參數arg
    cout << "參數" << arg << endl;
    //剩下未取出的參數繼續遞迴呼叫自已
    print(args...);
}
{% endhighlight %}

## 遞迴結束呼叫的函式

- 參數為空(因為可變參數清單已經全取出來，也沒參數可傳遞)
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

## 可變參數模板與bind與functional

- [functional][1]
- [bind][2]
- [l-value與r-value][3]
- [l-value參考與r-value參考][4]


寫一個函式可以接收任何種類的函式與不同數量的參數，函式與參數可為左值右值。

以下程式碼不支援函式多載(overload)

{% highlight c++ linenos %}
#include <iostream>
#include <functional>
using namespace std;
class Student {
public:
    void print(int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    }
};

void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}

template<typename Func, typename... Args>
auto callFunc(Func&& func, Args&&...args)
{
    auto f = bind(forward<Func>(func), forward<Args>(args)...);
    f();
    return f;
}

int main() {
    callFunc(print, 400, "Page not found.");
    //物件成員函式
    Student student;
    callFunc(&Student::print, student, 500, "Server error.");
    //函式為右值
    callFunc([](int code, const string& msg) {
            cout << "Lambda error code = " << code << " , Msg = " << msg << endl;
    }, 500, "Server error.");
    return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
Error code = 500 , Msg = Server error.
Lambda error code = 500 , Msg = Server error.
```


[1]: {% link _pages/c/c11/functional.md %}
[2]: {% link _pages/c/c11/bind.md %}
[3]: {% link _pages/c/c11/rvalue/l_r_value.md %}
[4]: {% link _pages/c/c11/rvalue/l_r_ref.md %}
[5]: {% link _pages/c/c11/forward.md %}