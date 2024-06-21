---
title: 指標運算
date: 2024-06-05
keywords: c++, Pointer arithmetic
---

指標 + 1，位址增減取決於記憶體位址中存放內容的資料型態。

為了方便看出來差異，以下的位址全轉型成long long長整數。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    char c;
    //印出char的大小
    cout << "sizeof(char) = " << sizeof(c) << endl;
    //印出變數c的位址
    cout << "c的地址 = " << (long long)&c << endl;
    //印出變數c的地址往後移動一格的位址，與變數c的位址相差為1byte
    cout << "c的地址 + 1 = " << (long long)(&c + 1) << endl;
    
    int i;
    //印出int的大小
    cout << "sizeof(int) = " << sizeof(i) << endl;
    //印出變數i的位址
    cout << "i的地址 = " << (long long)&i << endl;
    //印出變數i的地址往後移動一格的位址與變數i的位址相差為4byte
    cout << "i的地址 + 1 = " << (long long)(&i + 1) << endl;
    
    double d;
    //印出double的大小
    cout << "sizeof(double) = " << sizeof(d) << endl;
    //印出變數d位址
    cout << "d的地址 = " << (long long)&d << endl;
    //印出變數d的地址往後移動一格的位址，與變數d的位址相差為8byte
    cout << "d的地址 + 1 = " << (long long)(&d + 1) << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
sizeof(char) = 1
c的地址 = 140702053823595
c的地址 + 1 = 140702053823596
sizeof(int) = 4
i的地址 = 140702053823588
i的地址 + 1 = 140702053823592
sizeof(double) = 8
d的地址 = 140702053823576
d的地址 + 1 = 140702053823584
```


