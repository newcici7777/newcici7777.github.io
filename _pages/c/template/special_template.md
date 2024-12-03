---
title: 模板特製化
date: 2024-12-02
keywords: c++, template specialization
---

## 模板特製化template specialization

語法
```
template <> 
```
由template後面再加上尖括號<>，告知編譯器以下模板是有指定類型，參數類型都有符合才會執行。

## 函式模板

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
// 模板名為getMax，回傳類型為模板形參T，參數名為a與b
template <typename T>
T getMax(T a, T b) {
  if(a > b)
    return a;  // 若a比較大就把a傳回去
  else
    return b;  // 若b比較大就把b傳回去。
}
int main() {
  int a = 10;
  int b = 20;
  int max = getMax(a,b);
  cout << "max = " << max << endl;
  return 0;
}
{% endhighlight %}
```
max = 20
```

## 模板特製化

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
 public:
  void setScore(int score) {
    this->score = score;
  }
  int getScore() {
    return this->score;
  }
 private:
  int score;
};
// 函式模板
template <typename T>
T getMax(T a, T b) {
  if(a > b)
    return a;
  else
    return b;
}
// 模板特製化
template <>
Student getMax<Student>(Student s1, Student s2) {
  if(s1.getScore() > s2.getScore())
    return s1;
  else
    return s2;
}
int main() {
  int a = 10;
  int b = 20;
  int max = getMax(a,b);
  cout << "max = " << max << endl;
  Student s1;
  s1.setScore(78);
  cout << "s1 score = " << s1.getScore() << endl;
  Student s2;
  s2.setScore(50);
  cout << "s2 score = " << s2.getScore() << endl;
  // 呼叫模板特製化
  Student rtn = getMax(s1, s2);
  cout << "score = " << rtn.getScore()<< endl;
  return 0;
}
{% endhighlight %}
```
max = 20
s1 score = 78
s2 score = 50
score = 78
```

## 模板函式與特製化函式回傳值與參數要一致

若模板函式是使用T，特製化模板也要使用類型。若模板函式是使用T&，特製化模板則是使用類型參考&。否則若不一致，例:模板函式用T，特製化模板用類型參考&，將會產生錯誤error: no function template matches function template specialization。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
 public:
  void setScore(int score) {
    this->score = score;
  }
  int getScore() {
    return this->score;
  }
 private:
  int score;
};
template <typename T>
T& getMax(T& a, T& b) {
  if(a > b)
    return a;
  else
    return b;
}
template <>
Student& getMax<Student>(Student& s1, Student& s2) {
  if(s1.getScore() > s2.getScore())
    return s1;
  else
    return s2;
}
int main() {
  int a = 10;
  int b = 20;
  int& max = getMax(a,b);
  cout << "max = " << max << endl;
  Student s1;
  s1.setScore(78);
  cout << "s1 score = " << s1.getScore() << endl;
  Student s2;
  s2.setScore(50);
  cout << "s2 score = " << s2.getScore() << endl;
  // 呼叫模板特製化
  Student& ref = getMax(s1, s2);
  cout << "score = " << ref.getScore()<< endl;
  return 0;
}
{% endhighlight %}
```
max = 20
s1 score = 78
s2 score = 50
score = 78
```