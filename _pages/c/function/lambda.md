---
title: lambda
date: 2024-10-29
keywords: c++, lambda
---

Prerequisites:

[函式指標][1]

[auto與函式][2]


lambda又稱匿名函式，也就是`沒有名字`的函式。

## 語法介紹

```
[]()->Int{
	return 0;
}
```

[] 獲取外部的變數值或參考

() 函式參數

-> 函式回傳值型態，若為void可以不寫，

{} 函式主體

### []傳入外部變數值或參考

auto與函式要先看過才看的懂以下程式碼。


#### 傳變數值

以下程式碼是把外部變數name透過[name]的方式，傳入lambda。

若要傳多個，用逗號。

語法
```
[name, age, ...]
```

{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 20;
    auto f1 = [name](){
        cout << "name = " << name << endl;
    };
    f1();
    return 0;
}
{% endhighlight %}

```
name = Mary
```

#### 傳變數參考

#### 傳值

#### 傳參考

#### 混合傳

#### 不傳任何變數


## 函式轉成lambda

### 轉換前

以下程式碼有fun()的函式，參數為int型別的x與y，回傳值型別為int。

{% highlight c++ linenos %}
int fun(int x, int y) {
    return x + y;
}
int main() {
    cout << "x + y = " << fun(10, 100) << endl;
    return 0;
}
{% endhighlight %}
```
x + y = 110
```

### 轉換後

使用auto宣告函式型態，f為函式變數名。

{% highlight c++ linenos %}
int main() {
    auto f = [](int x, int y) -> int {
        return x + y;
    };
    cout << "x + y = " << f(10, 100) << endl;
    return 0;
}
{% endhighlight %}
```
x + y = 110
```

## 比對fun函式跟lambda匿名函式
{% highlight c++ linenos %}
int fun(int x, int y) {
    return x + y;
}
{% endhighlight %}

{% highlight c++ linenos %}
[](int x, int y) -> int {
        return x + y;
    };
{% endhighlight %}

[1]: {% link _pages/c/function/functionPointer.md %}
[2]: {% link _pages/c/function/auto_func.md %}