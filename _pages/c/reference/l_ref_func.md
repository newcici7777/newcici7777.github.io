---
title: 等號左邊的函式
date: 2024-07-02
keywords: c++, l-value reference, r-value reference
---

Prerequisites:

- [l-value與r-value][1]
- [l-value參考與r-value參考][2]
- [引數][3]
- [函式傳回值是參考][4]

## 函式傳回值是參考別名

以下程式碼是將全域變數x的參考別名作為傳回值。
{% highlight c++ linenos %}
//全域變數
int x = 10;
//返回全域變數x的參考別名
int& setX(){
    return x;
}
{% endhighlight %}


將`setX() = 99`可以視作為`int& x_ref = 99;`

二者是相同意思。

{% highlight c++ linenos %}
setX() = 99;
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
//全域變數
int x = 10;
//返回全域變數x的參考
int& setX(){
    return x;
}
int main() {
    cout << "Before x = " << x << endl;
    //將x全域變數參考，設值成99
    setX() = 99;
    cout << "After x = " << x << endl;
    return 0;
}
{% endhighlight %}

```
Before x = 10
After x = 99
```

## 不能傳回區域變數的參考

以下程式碼會出錯，因為區域變數x在函式結束的時候就會被系統記憶體釋放，無法作為參考。
{% highlight c++ linenos %}
int& setX(){
    int x = 10;
    return x;
}
{% endhighlight %}

## 引數作為參考

以下程式碼將main()函式中的x變數傳進setX()的函式，並回傳x變數的匿名參考別名。

將x的參考設值99。

{% highlight c++ linenos %}
//回傳值為參考別名
//參數r為x的參考別名
int& setX(int& r){
    return r;
}
int main() {
    int x = 10;
    cout << "Before x = " << x << endl;
    //x傳入函式，回傳x的參考
    setX(x) = 99;
    cout << "After x = " << x << endl;
    return 0;
}
{% endhighlight %}

```
Before x = 10
After x = 99
```

## 函式不想放在等號左邊

若不想被人修改傳回值，可將傳回值設為const，但函式呼叫者也需要加上const。

函式語法
```
const 資料型態& 函式名(資料型態& 原始變數);
const int& getValue(int& z);
```

函式呼叫者
```
const 資料型態& 別名 = 函式名(原始變數);
const int& y = getValue(x);
```

以下程式碼將編譯錯誤，函式無法放在等號左邊。
```
getValue(x) = 100;
```

以下為完整程式碼

{% highlight c++ linenos %}
const int& getValue(int& z){
    z++;
    return z;
}
int main() {
    int x = 10;
    const int& y = getValue(x);
    cout << "x = " << x << endl;
    cout << "y = " << y << endl;
    return 0;
}
{% endhighlight %}

```
x = 11
y = 11
```


[1]: {% link _pages/c/reference/l_r_value.md %}
[2]: {% link _pages/c/reference/l_r_ref.md %}
[3]: {% link _pages/c/basic/param.md %}
[4]: {% link _pages/c/function/func_return_ref.md %}