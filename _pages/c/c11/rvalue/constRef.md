---
title: const與參考
date: 2024-06-24
keywords: c++, const reference
---

Prerequisites:

- [l-value與r-value][1]

- [l-value參考與r-value參考][2]

## const參考

const參考可以放左值(l-value)，也可以放右值(r-value)，也可以放常數。

### const參考放左值

{% highlight c++ linenos %}
int main() {
    // i 是l-value
    int i = 100; // 100 is r-value
    const int& ref_i = i;
    cout << "ref_i = " << ref_i << endl;
    return 0;
}
{% endhighlight %}

```
ref_i = 100
```

### const參考放右值

{% highlight c++ linenos %}
int main() {
    const int& ref_r = 100; // 100 is r-value
    cout << "ref_r = " << ref_i << endl;
    return 0;
}
{% endhighlight %}
```
ref_r = 100
```

### const參考放右值&&變數名

{% highlight c++ linenos %}
int main() {
    int&& r = 100;
    const int& ref_r = r;
    cout << "ref_r = " << ref_i << endl;
    return 0;
}
{% endhighlight %}
```
ref_r = 100
```

### const參考放常數名
{% highlight c++ linenos %}
int main() {
    //con 是常數
    const int con = 100;
    const int& ref = con;
    cout << "ref = " << ref << endl;
    return 0;
}
{% endhighlight %}

```
ref = 100
```

### const參考放常數值

{% highlight c++ linenos %}
int main() {
    const int& ref = 100; //100是常數
    cout << "ref = " << ref << endl;
    return 0;
}
{% endhighlight %}

```
ref = 100
```
## 函式參數為const參考

{% highlight c++ linenos %}
void func(const int& ref) {
    cout << "ref = " << ref << endl;
}
int main() {
    // i is l-value
    int i = 10;
    //l-value
    func(i);
    
    //r-value
    int&& r = 20;
    func(r);
    
    func(100);// 常數 100 , 100 is r-value
    func('A');// 常數 'A' , A is r-value
    return 0;
}
{% endhighlight %}

```
ref = 10
ref = 20
ref = 100
ref = 65
```


[1]: {% link _pages/c/c11/rvalue/l_r_value.md %}
[2]: {% link _pages/c/c11/rvalue/l_r_ref.md %}
