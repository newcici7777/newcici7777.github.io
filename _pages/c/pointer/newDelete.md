---
title: new/delete
date: 2024-05-30
keywords: c++, new, delete, Dynamic memory allocation
---

### new

宣告指標變數，使用new在Heap區段申請記憶體空間，申請成功會回傳記憶體占用的開始地址。

```
類型* 指標變數 = new 類型(初始值);
```
{% highlight c++ linenos %}
int main() {
    int* p = new int(30);
    cout << "地址是 = " << p << endl;
    cout << "地址中的內容是 = " << *p << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
地址是 = 0x600000004010
地址中的內容是 = 30
```

### delete
在Heap區段申請記憶體空間，不用時需使用delete對記憶體進行釋放，否則會產生memory leak，記憶體一直沒被釋放，記憶體愈占愈多，最後就會當機。

釋放記憶體地址語法如下，delete後要把指標變數設為nullptr(也可以設0)，代表沒有指向任何地址的意思。

```
delete 指標變數;
指標變數 = nullptr;
```

{% highlight c++ linenos %}
int main() {
    int* p = new int(30);
    delete p;
    p = nullptr;
    return 0;
}
{% endhighlight %}