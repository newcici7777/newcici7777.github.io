---
title: 成員變數是類別
date: 2024-10-27
keywords: c++, class 
---
在Java的世界中，只有new的那一刻才會呼叫類別的建構子。

但在C++的世界中，Family m_family;就會呼叫類別的建構子，並且在記憶體建立此物件的位址。

{% highlight c++ linenos %}
class Student {
public:
    Family m_family;
};
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Family {
public:
    string mon;
    string dad;
    Family(){
        cout << "Family 建構子" << endl;
    }
};
class Student {
public:
    Family m_family;
};
int main() {
    Student student;
    return 0;
}
{% endhighlight %}

```
Family 建構子
```

由執行結果可以知道，建立student物件的時候，就會建立Family物件。