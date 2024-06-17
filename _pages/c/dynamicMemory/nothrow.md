---
title: 記憶體不足
date: 2024-06-15
keywords: c++, dynamic arrays, nothrow
---

## std::nothrow

可以判斷若記憶體太小導致記憶體空間分配失數，會回傳nullptr。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int *arr = new (std::nothrow)int[10000000000000001];
    if(arr == nullptr){
        cout << "記憶體配置失敗";
    } else {
        arr[10000000000000001] = 100;
        delete[] arr;
        arr = nullptr;
    }
    return 0;
}
{% endhighlight %}

```
執行結果
記憶體配置失敗
```