---
title: 函式多載
date: 2024-10-14
keywords: c++, function overload
---

## 多載override

多載是指相同函式名，完成相似的工作。

函式名相同，回傳值相同，但參數的型態或數量不同，編譯時，編譯器會根據引數類型與函式參數類型，引數的數量與函式參數的數量，進行匹配。

- int func(short a, string b);
- int func(int a, string b);
- int func(double a, string b);
- int func(int a, string b, int c);
- int func(string a, int b);

{% highlight c++ linenos %}
void max(float a, float b) {
    cout << "比較float" << endl;
    if(a > b)
        cout << "a大" << endl;
    else
        cout << "b大" << endl;
}
void max(int a, int b) {
    cout << "比較int" << endl;
    if(a > b)
        cout << "a大" << endl;
    else
        cout << "b大" << endl;
}
int main() {
    float a1 = 100.5f;
    float b1 = 100.3f;
    max(a1, b1);
    
    int a2 = 20;
    int b2 = 30;
    max(a2, b2);
    return 0;
}
{% endhighlight %}
```
比較float
a大
比較int
b大
```

## 多載注意事項

### 函式名相同，參數型態與數量相同，但一個參數有const，這樣不是函式多載。

以下函式名、參數型態與數量相同，但有一個是const，這樣不是函式多載。

{% highlight c++ linenos %}
void max(float a, float b) {
    cout << "比較float" << endl;
    if(a > b)
        cout << "a大" << endl;
    else
        cout << "b大" << endl;
}
void max(const float a, float b) {
    cout << "比較float" << endl;
    if(a > b)
        cout << "a大" << endl;
    else
        cout << "b大" << endl;
}
{% endhighlight %}

### 返回值要相同

### 函式名相同，參數型態與數量相同，但一個參數是參考，編譯器不知道要使用那一個

{% highlight c++ linenos %}
void max(float a, float b) {
    cout << "比較float" << endl;
    if(a > b)
        cout << "a大" << endl;
    else
        cout << "b大" << endl;
}
void max(float& a, float b) {
    cout << "比較float" << endl;
    if(a > b)
        cout << "a大" << endl;
    else
        cout << "b大" << endl;
}
int main() {
    float a1 = 100.5f;
    float b1 = 100.3f;
    max(a1, b1);
    return 0;
}
{% endhighlight %}

### 使用預設值，編譯器不知道要使用那一個函式

以下有二個相同名稱的函式，一個是三個參數(但有預設值)，一個是二個參數(沒預設值)，若使用二個引數呼叫，編譯器不知道要使用那一個函式。

{% highlight c++ linenos %}
void func1(int error_code, string msg1 = "這是預設值1", const string& msg2 = "這是預設值2") {
    cout << "Error code: " << error_code << ", ";
    cout << "Msg1: " << msg1 << ", ";
    cout << "Msg2: " << msg2 << endl;
}
void func1(int error_code, string msg1) {
    cout << "Error code: " << error_code << ", ";
    cout << "Msg1: " << msg1 << ", ";
}
int main() {
    func1(500,"Server error!");
    return 0;
}
{% endhighlight %}
