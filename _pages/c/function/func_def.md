---
title: 函式宣告與定義
date: 2024-11-01
keywords: c++, function declaration, function definition
---

## 函式宣告(declaration)

沒有程式碼，最後面有冒號;

告知編譯器有一個函式名字為print，傳回值型態為void，參數類型有int與const string&，之後會有程式碼實作。

```
void print(int code,const string& msg);
```

## 函式定義(definition)

有程式碼。

{% highlight c++ linenos %}
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
{% endhighlight %}

## 呼叫函式

```
print(500, "Server Error.");
```

## 函式宣告與函式定義

{% highlight c++ linenos %}
//函式宣告
void print(int code,const string& msg);
int main() {
	//呼叫函式
    print(500, "Server Error.");
    return 0;
}
//函式定義
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
{% endhighlight %}

```
Error code = 500 , Msg = Server Error.
```

## 函式宣告與定義合在一起

在main()主程式前宣告函式，並且實作程式碼，就是函式宣告與定義結合一起，有實作程式碼一律稱之函式定義。

{% highlight c++ linenos %}
//函式定義
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
int main() {
    print(500, "Server Error.");
    return 0;
}
{% endhighlight %}

```
Error code = 500 , Msg = Server Error.
```