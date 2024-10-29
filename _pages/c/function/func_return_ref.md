---
title: 函式傳回值是參考
date: 2024-07-02
keywords: c++, l-value reference, r-value reference
---

Prerequisites:

- [傳參考][1]

- [函式傳回值是值][2]


若傳回值是參考別名，將不會拷貝傳回值到暫存器或stack。

函式宣告語法

```
資料型態& 函式名(資料型態& 參考別名)
int& getValue(int& z);
```

函式呼叫方式

y是原始變數的參考別名。
```
資料型態& 參考別名 = 函式(原始變數);
int& y = getValue(原始變數);
```

以下程式碼z與y都是x(原始變數)的參考別名，z與y與x三者都指向相同記憶體位址。

完整程式碼
{% highlight c++ linenos %}
int& getValue(int& z){
    z++;
    return z;
}
int main() {
    int x = 10;
    int& y = getValue(x);
    cout << "x = " << x << endl;
    cout << "y = " << y << endl;
    return 0;
}
{% endhighlight %}

```
x = 11
y = 11
```


[1]: {% link _pages/c/function/callByRef.md %}
[2]: {% link _pages/c/function/callByValue.md %}#函式傳回值是值