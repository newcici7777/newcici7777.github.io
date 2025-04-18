---
title: 指標模板
date: 2025-04-16
keywords: c++, template, pointer template
---
函式的參數是指標:
{% highlight c++ linenos %}
void func1(int* param1) {
  cout << "param1=" << *param1 << endl;
}
int main() {
  int var1 = 10;
  int* p = &var1;
  func1(p);
  return 0;
}
{% endhighlight %}

可以改成以下方式:
{% highlight c++ linenos %}
template <typename T>
void func1(T param1) {
  cout << "param1=" << *param1 << endl;
}
int main() {
  int var1 = 10;
  int* p = &var1;
  func1<int*>(p);
  return 0;
}
{% endhighlight %}

也可以改成以下方式:  
明確定義參數是指標類型，但呼叫函式宣告的型別不能為`<int*>`，要為`<int>`，告訴編譯器T是int型別，若函式宣告的型別為int指標型別`<int*>`，編譯器會把T\*再加上一個\*，最後就會變成T\*\*，變成指標的指標，但實際上傳入的參數是指標。  
{% highlight c++ linenos %}
template <typename T>
void func1(T* param1) {
  cout << "param1=" << *param1 << endl;
}
int main() {
  int var1 = 10;
  int* p = &var1;
  func1<int>(p);
  return 0;
}
{% endhighlight %}

以下的Queue中的T，可以是基本型別或指標或類別，沒有限定型別。  
{% highlight c++ linenos %}
template<typename T>
class MyQueue {
private:
    std::queue<T> q;
public:
    void push(const T new_value) {
        q.push(new_value);
    }
};
{% endhighlight %}

指標放入queue
{% highlight c++ linenos %}
int main() {
  MyQueue<int*> my_q;
  int var1 = 10;
  int* p = &var1;
  my_q.push(p);
  return 0;
}
{% endhighlight %}

基本型別放入queue
{% highlight c++ linenos %}
int main() {
  MyQueue<int> my_q;
  int var1 = 10;
  my_q.push(var1);
  return 0;
}
{% endhighlight %}
