---
title: 函式
date: 2024-10-14
keywords: c++, function 
---

## 參數預設值

### 參數預設值程式碼

有預設值，呼叫函式時就可以不用代入參數。

{% highlight c++ linenos %}
void func1(const string& msg = "這是預設值") {
    cout << msg << endl;
}
int main() {
    func1("test");
    func1();
    return 0;
}
{% endhighlight %}

```
test
這是預設值test
這是預設值
```

### 宣告函式與定義函式分開，函式預設值只能在宣告

- 何謂宣告函式？

宣告就是告訴編譯器我有一個函式藍圖模型(尚未實作)，又稱函式原型(prototype)，在程式的某處會引入此函式。

- 何謂定義函式？

函式全部的程式碼。

- 何謂呼叫函式？

程式執行到呼叫函式，程式的執行會移轉到函式定義(函式全部的程式碼)。

{% highlight c++ linenos %}
//宣告函式
void func1();
int main() {
	//呼叫函式
    func1();
    return 0;
}
//定義函式
void func1() {
	cout << "test" << endl;
}
{% endhighlight %}

- 函式預設值只能在宣告

{% highlight c++ linenos %}
//宣告函式
void func1(const string& msg = "這是預設值");
int main() {
	//呼叫函式
    func1("test");
    func1();
    return 0;
}
//定義函式
void func1(const string& msg) {
    cout << msg << endl;
}
{% endhighlight %}

```
test
這是預設值
```

### 若有一個參數有預設值，排在它後面的參數也要有預設值

參數msg2排在參數msg1後面，參數msg1已設置預設值，若參數msg2不設置預設值會編譯錯誤。

呼叫函式，有預設值的參數，可以不用設置引數。

{% highlight c++ linenos %}
void func1(int error_code, const string& msg1 = "這是預設值1", const string& msg2 = "這是預設值2") {
    cout << "Error code: " << error_code << ", ";
    cout << "Msg1: " << msg1 << ", ";
    cout << "Msg2: " << msg2 << endl;
}
int main() {
    func1(500, "Server Error!", "Server has some error.");
    func1(404, "Not Found!");
    func1(200);
    return 0;
}
{% endhighlight %}

```
Error code: 500, Msg1: Server Error!, Msg2: Server has some error.
Error code: 404, Msg1: Not Found!, Msg2: 這是預設值2
Error code: 200, Msg1: 這是預設值1, Msg2: 這是預設值2
```

### 若某個參數設值，排在它之前的參數也要設值

下例中，傳值給第3個參數，前面2個參數都要設置。

```
    func1(500, "Server Error!", "Server has some error.");
    func1(404, "Not Found!");
```


{% highlight c++ linenos %}
void func1(int error_code, const string& msg1 = "這是預設值1", const string& msg2 = "這是預設值2") {
    cout << "Error code: " << error_code << ", ";
    cout << "Msg1: " << msg1 << ", ";
    cout << "Msg2: " << msg2 << endl;
}
int main() {
    func1(500, "Server Error!", "Server has some error.");
    func1(404, "Not Found!");
    return 0;
}
{% endhighlight %}


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

## 內嵌函式inline

函式會占記憶體空間，主程式呼叫函式時會跳到函式占用的記憶體位址，待函式結尾再跳回主程式，為了節省跳來跳去的執行時間，可在函式前面加上inline，編譯器一看到inline就會把函式拷貝一份插入在主程式之中。

在以下程式碼print()最前面加上inline，使print()函式變成內嵌函式。

{% highlight c++ linenos %}
inline void print(string s) {
    cout << s << endl;
}
int main() {
    print("test");
    print("abcdef");
    print("aaaa");
    return 0;
}    
{% endhighlight %}

轉變內嵌函式，程式在執行時就會變成以下這樣

{% highlight c++ linenos %}
int main() {
    {
        string s = "test";
        cout << s << endl;
    }
    {
        string s = "abcdef";
        cout << s << endl;
    }
    {
        string s = "aaaa";
        cout << s << endl;
    }
    return 0;
}    
{% endhighlight %}

若是呼叫print() 1000次就會copy程式碼1000份在主程式中，占用記憶體空間。(變數也是占記憶體位址)