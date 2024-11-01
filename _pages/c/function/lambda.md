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

注意！傳值的方式是不能修改外部變數。

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

傳參考是可以修改變數的值

若要傳多個，用逗號。

語法
```
[&name, &age, ...]
```

{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    auto f1 = [&name](){
        name = "Bill";
        cout << "name = " << name << endl;
    };
    f1();
    return 0;
}
{% endhighlight %}
```
name = Bill
```
#### 傳值=

若lambda中有用到外部變數，就會直接以值的方式把有用到的外部變數傳入lambda

語法
```
[=]
```
{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 18;
    auto f1 = [=](){
        cout << "name = " << name << endl;
        cout << "age = " << age << endl;
    };
    f1();
    return 0;
}
{% endhighlight %}
```
name = Mary
age = 18
```

#### 傳參考&

若lambda中有用到外部變數，就會直接以參考的方式把有用到的外部變數傳入lambda

傳參考是可以修改變數的值

語法
```
[&]
```
{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 18;
    auto f1 = [&](){
        name = "Bill";
        age = 30;
        cout << "name = " << name << endl;
        cout << "age = " << age << endl;
    };
    f1();
    return 0;
}
{% endhighlight %}
```
name = Bill
age = 30
```

#### 混合傳

語法
```
[&, name]
```
除了name變數是傳值的方式，其它lambda中有用到的變數一律傳參考的方式代入。
{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 18;
    auto f1 = [&, name](){
        age = 30;
        cout << "name = " << name << endl;
        cout << "age = " << age << endl;
    };
    f1();
    return 0;
}
{% endhighlight %}
```
name = Mary
age = 30
```

語法
```
[=, &name]
```
除了name變數是傳參考的方式，其它lambda中有用到的變數一律傳值的方式代入。
{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 18;
    auto f1 = [=, &name](){
        name = "Tom";
        cout << "name = " << name << endl;
        cout << "age = " << age << endl;
    };
    f1();
    return 0;
}
{% endhighlight %}
```
name = Tom
age = 18
```

#### 不傳任何外部變數
語法
```
[]
```

外部變數都不能使用

{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 18;
    auto f1 = [](){
        //cout << "name = " << name << endl;
        //cout << "age = " << age << endl;
    };
    f1();
    return 0;
}
{% endhighlight %}

### 傳參數

語法
```
(參數型態 參數1, 參數型態2 參數2, ...)
(string address)
```

以下程式碼設定lambda的參數，呼叫函式的時候代入參數。

{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 18;
    auto f1 = [](string address){
        cout << "address = " << address << endl;
    };
    f1("Taiwan Taipei ....");
    return 0;
}
{% endhighlight %}

### 回傳值型態

若回傳型態為void，可以省略以下語法，也可以不寫，lambda會自動推導回值型態

```
->型態
```
{% highlight c++ linenos %}
int main() {
    string name = "Mary";
    int age = 18;
    auto f1 = [](string address) -> int{
        cout << "address = " << address << endl;
        return 100;
    };
    int val = f1("Taiwan Taipei ....");
    cout << "val = " << val << endl;
    return 0;
}
{% endhighlight %}
```
address = Taiwan Taipei ....
val = 100
```

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