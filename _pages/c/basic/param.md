---
title: 參數與引數
date: 2024-10-14
keywords: c++,  
---

Prerequisites:

[維基參數和引數][1]

## 引數 Argument

傳給函式的變數。

{% highlight c++ linenos %}
int main() {
  int var1 = 10;
  //將var1傳到func1()函式中，而var1就是引數Argument
  func1(var1);
  return 0;
}
{% endhighlight %}

## 參數 Parameter

函式接收到的變數。

{% highlight c++ linenos %}
//函式接收到來自外面傳進來的變數，變數名param1，而param1就是參數
void func1(int param1) {
  cout << "param1=" << param1 << endl;
}
{% endhighlight %}


[1]: https://zh.wikipedia.org/wiki/%E5%8F%83%E6%95%B8_(%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88)