---
title: 類別中函式模板
date: 2024-12-02
keywords: c++, function template in class
---

## 建構子

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class MyArray{
 public:
  template <typename T>
  // 建構子的參數為模板類型參數T
  MyArray(T element) {
  cout << "element : " << element << endl;
  }
};
int main() {
  // 引數為整數
  MyArray myArray1(10);
  // 引數為字串  
  MyArray myArray2("abcdefg");
  // 引數為double
  MyArray myArray3(1000.555);
  return 0;
}
{% endhighlight %}
```
element : 10
element : abcdefg
element : 1000.55
```

## 成員函式

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
using namespace std;
class MyArray{
 public:
  template <typename T>
  MyArray(T element) {
  cout << "element : " << element << endl;
  }
  // 建立成員函式模板printMsg，傳回值為void，參數為模板類型參數T
  template <typename T>
  void printMsg(T msg) {
  cout << "msg : " << msg << endl;
  }
};
int main() {
  MyArray myArray1(10);
  // 引數為字串
  myArray1.printMsg("Data Not Found!");
  // 引數為數字
  myArray1.printMsg(404);
  // 引數為字元
  myArray1.printMsg('$');
  return 0;
}
{% endhighlight %}
```
element : 10
msg : Data Not Found!
msg : 404
msg : $
```
