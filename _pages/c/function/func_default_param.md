---
title: 函式參數預設值
date: 2024-10-14
keywords: c++, function 
---

## 參數預設值

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

## 宣告函式與定義函式分開，函式預設值只能在宣告

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

## 若有一個參數有預設值，排在它後面的參數也要有預設值

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

## 若某個參數設值，排在它之前的參數也要設值

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


