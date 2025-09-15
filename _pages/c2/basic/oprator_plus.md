---
title: 自增自減
date: 2025-09-12
keywords: c++, c, 
---
## i++與++i

先看以下代碼

{% highlight c++ linenos %}
int main() {
    int i = 10;
    i = i + 1;
    return 0;
}
{% endhighlight %}

第2行，宣告i變數，初始化的值為10。

第3行，將i變數的值+1，再賦值給原來的i變數。

可以將第3行取代為以下程式碼∶

i++

{% highlight c++ linenos %}
int main() {
    int i = 10;
    i++;
    return 0;
}
{% endhighlight %}

++i

```cpp
int main() {
    int i = 10;
    ++i;
    return 0;
}
```

執行結果都是一樣的。

```
i = 11
```

## var = i++;

先看以下代碼∶

{% highlight c++ linenos %}
int main() {
    int i = 10;
    int var;
    var = i;
    i = i + 1;
    cout << "i = " << i << endl;
    cout << "var = " << var << endl;
    return 0;
}
{% endhighlight %}

第2行，宣告i變數，初始化的值為10。

第3行，宣告var變數，沒初始化。

第4行，將i變數的值10，賦值給var變數。

第5行，將i變數的值+1，再賦值給原來的i變數。

第6行，印出i變數的值。

第7行，印出var變數的值。

印出結果

```
i = 11
var = 10
```

將第4行-第5行程式碼

```cpp
    var = i;
    i = i + 1;
```

取代成

```
    var = i++;
```

意思就是

1. 將i變數的值10，賦值給var變數。
2. 將i變數的值+1，再賦值給原來的i變數。

完整程式碼

{% highlight c++ linenos %}
int main() {
    int i = 10;
    int var;
//    var = i;
//    i = i + 1;
    var = i++;
    cout << "i = " << i << endl;
    cout << "var = " << var << endl;
    return 0;
}
{% endhighlight %}

執行結果

```
i = 11
var = 10
```

## var = ++i;

將最上面程式碼，第4行與第5行顛倒。

{% highlight c++ linenos %}
int main() {
    int i = 10;
    int var;
    i = i + 1;
    var = i;
    cout << "i = " << i << endl;
    cout << "var = " << var << endl;
    return 0;
}
{% endhighlight %}

第4行，將i變數的值+1，再賦值給原來的i變數。

第5行，將i變數的值11，賦值給var變數。

執行結果

```
i = 11
var = 11
```

將第4行-第5行程式碼

```
    i = i + 1;
    var = i;
```

取代成

```
    var = ++i;
```

意思就是

1. 將i變數的值+1，再賦值給原來的i變數。
2. 將i變數的值11，賦值給var變數。

完整程式碼

{% highlight c++ linenos %}
int main() {
    int i = 10;
    int var;
//    i = i + 1;
//    var = i;
    var = ++i;
    cout << "i = " << i << endl;
    cout << "var = " <<var << endl;
    return 0;
}
{% endhighlight %}



