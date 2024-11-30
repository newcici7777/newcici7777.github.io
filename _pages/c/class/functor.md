---
title: operator()物件函式
date: 2024-10-28
keywords: c++, operator(),Functor
---

## 物件函式Functor

物件名本身就是函式名，但使用方法像呼叫函式一樣。

必須在類別裡面定義

以下回傳值型態與參數可自定義，沒有限制，但函式名必須為`operator()`
{% highlight c++ linenos %}
    回傳值型態 operator()(參數) {
        cout << "要做的事" << endl;
    }
{% endhighlight %}

呼叫物件函式步驟
- 建立物件
- 呼叫物件函式
{% highlight c++ linenos %}
    //建立物件
    Student student;
    //呼叫物件函式，注意!這裡不是呼叫建構子
    student();
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
    string m_name;
    void operator()() {
        cout << "Hello" << endl;
    }
};
int main() {
    //建立物件
    Student student;
    //呼叫物件函式，注意!這裡不是呼叫建構子
    student();
    return 0;
}
{% endhighlight %}

```
Hello
```

## 全域函式與物件函式同名如何執行？

有一個全域函式名為student()

有一個物件函式名為student()

如何區別，使用::範圍運算子，來進行區別。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
    string m_name;
    void operator()() {
        cout << "Hello" << endl;
    }
};
void student() {
    cout << "abcdefghijkl......" << endl;
}
int main() {
    //建立物件
    Student student;
    //呼叫全域student()
    ::student();
    //呼叫物件函式
    student();
    return 0;
}
{% endhighlight %}
```
abcdefghijkl......
Hello
```