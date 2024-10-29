---
title: const與參考
date: 2024-06-24
keywords: c++, const reference
---

Prerequisites:

- [l-value與r-value][1]

- [l-value參考與r-value參考][2]

## const參考與r-value

下方程式可以指派常數100到常數參考ref1。

```
 const int& ref1 = 100;
```

如果常數指派到參考，卻沒加上const修飾子，就會編譯錯誤，因為參考不支援常數。
> Non-const lvalue reference to type 'int' cannot bind to a temporary of type 'int'

```
int& ref1 = 100;
```

若使用const參考，編譯器會產生一個臨時變數temp，把常數100指派到臨時變數中，再把臨時變數指派給參考。
```
    int temp = 100;
    const int& ref = temp;
```

## const參考與自動轉型

當[引數][4]為常數給函式，除了[r-value參考][3]可以接收常數，const參考也可以接收常數，二者功能相同。

const參考會自動轉型，下面例子將'C'自動轉型為int

{% highlight c++ linenos %}
int main() {
    const int& ref1 = 100;
    cout << "ref1 = " << ref1 << endl;
    const int& ref2 = 'C';
    cout << "ref2 = " << ref2 << endl;
    return 0;
}
{% endhighlight %}

```
ref1 = 100
ref2 = 67
```

## 函式參數為const參考

{% highlight c++ linenos %}
void func(const int& ref) {
    cout << "ref = " << ref << endl;
}
int main() {
    func(100); // 常數 100 , 100 is r-value
    func('A'); // 常數 'A' , A is r-value 
    return 0;
}
{% endhighlight %}

```
ref = 100
ref = 65
```


[1]: {% link _pages/c/reference/l_r_value.md %}
[2]: {% link _pages/c/reference/l_r_ref.md %}
[3]: {% link _pages/c/reference/l_r_ref.md %}#引數與參數r-value參考資料型態不同
[4]: {% link _pages/c/basic/param.md %}