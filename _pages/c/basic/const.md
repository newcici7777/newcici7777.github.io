---
title: const常數與唯讀
date: 2025-09-10
keywords: c++, const
---
## const常數
在main()主程式以前，設定const類型、常數名與值，後面(尾部)有分號`;`。<br>
語法
```
const 類型 常數名 = 值;
int main() {
  return 0;
}
```

{% highlight c++ linenos %}
const double PI = 3.14159;
int main() {
  cout << PI << endl;
  return 0;
}
{% endhighlight %}

### 無法修改const
以下程式碼會編譯錯誤，常數不可再被修改。
{% highlight c++ linenos %}
const double PI = 3.14159;
int main() {
  PI = 2.55;
  cout << PI << endl;
  return 0;
}
{% endhighlight %}

### 無法重覆宣告const
以下程式碼會編譯錯誤，常數不可重覆宣告。
{% highlight c++ linenos %}
const double PI = 3.14159;
const double PI = 1.666;
int main() {
  cout << PI << endl;
  return 0;
}
{% endhighlight %}

### 運行階段
const編譯的時候不會有值，只有運行階段才會有值。

以下會編譯錯誤，因為編譯的時候找不到ARR_SIZE的值。
{% highlight c++ linenos %}
const ARR_SIZE = 10;
int main() {
  int arr[ARR_SIZE];
  return 0;
}
{% endhighlight %}

使用#define，編譯「前」就會有值，#define的中文叫前置處理，大陸的中文是預編譯，意思是編譯之前就已經有常數值。<br>
注意！define最後面不能有分號`;`，指派10給ARR_SIZE，中間也沒有等號`=`。<br>
以下編譯正確。<br>
{% highlight c++ linenos %}
#define ARR_SIZE 10
int main() {
  int arr[ARR_SIZE];
  return 0;
}
{% endhighlight %}

## const與變數
這邊的const不是常數，而是讓變數只能讀，不能修改。

`const 變數`代表只能讀取變數，不可修改變數「內容」。

### const指標與函式參數，無法修改外部位址
之前在[const與指標][1]的文件中，提到const指標可以修改記憶體位址。<br>

但在函式參數中，只要是指標，就無法修改記憶體位址。<br>

以下程式，不管change_addr()函式的參數是一般指標或是const指標，都無法修改main()函式中的i變數記憶體位址。<br>

一般指標<br>
{% highlight c++ linenos %}
void change_addr(int* x) {
  x = &global_i;
  printf("chage_addr() i addr = %p \n",x);
}
int main() {
  int i = 10;
  printf("before i addr = %p \n",i);
  change_addr(&i);
  printf("after i addr = %p \n",i);
  return 0;
}
{% endhighlight %}
```
before i addr = 0x1000 
chage_addr() i addr = 0x2000
after i addr = 0x1000
```

const指標<br>
{% highlight c++ linenos %}
void change_addr(const int* x) {
  x = &global_i;
  printf("chage_addr() i addr = %p \n",x);
}
int main() {
  int i = 10;
  printf("before i addr = %p \n",i);
  change_addr(&i);
  printf("after i addr = %p \n",i);
  return 0;
}
{% endhighlight %}
```
before i addr = 0x1000 
chage_addr() i addr = 0x2000
after i addr = 0x1000
```
### const與函式參數指標
設計一個函式只能讀取參數，但不能修改參數。<br>

以下編譯失敗，因為只能讀取，無法修改參數。<br>
{% highlight java linenos %}
void read_only(const int* p) {
  * p = 55;  // 這邊編譯失敗，不能修改記憶體位址的內容
  cout << * p << endl;
}
int main() {
  int i = 55;
  read_only(&i);
  return 0;
}
{% endhighlight %}

### const與物件
以下程式碼，在compare()函式中，參數使用const，代表只能讀取，不能修改。
{% highlight c++ linenos %}
void compare(const Student* s2);
{% endhighlight %}

不可以在函式中進行如下操作:<br>
不能修改名字跟分數，編譯錯誤。<br>
{% highlight c++ linenos %}
void compare(const Student* s2) {
	s2->name_ = "Alice";
	s2->score_ = 100;
}
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
class Student{
public:
  char* name_;
  int score_;
  Student() {}
  Student(char* name, int score): name_(name), score_(score) {}
  void compare(const Student* s2) {
    if (this->score_ > s2->score_) {
      cout << "本身score比較大" << endl;
    } else {
      cout << "s2 score比較大" << endl;
    }
  }
};
int main() {
  Student s1("Mary", 80);
  Student s2("Bill", 90);
  s1.compare(&s2);
  return 0;
}
{% endhighlight %}

### const與成員函式
const在函式名()後面，代表的是，無法修改this，this是本身的物件

const放置位置，在函式後面。
```
傳回值 成員函式(參數) const {
}
```
以下函式可以修改s2參數。
{% highlight c++ linenos %}
class Student{
public:
  char* name_;
  int score_;
  Student() {}
  Student(char* name, int score): name_(name), score_(score) {}
  void compare(Student* s2) const {
    s2->name_ = "Alice";
    s2->score_ = 100;
    if (this->score_ > s2->score_) {
      cout << "本身score比較大" << endl;
    } else {
      cout << "s2 score比較大" << endl;
    }
  }
};
int main() {
  Student s1("Mary", 80);
  Student s2("Bill", 90);
  s1.compare(&s2);
  printf("s2 name = %s, score = %d\n", s2.name_, s2.score_);
  return 0;
}
{% endhighlight %}
```
s2 score比較大
s2 name = Alice, score = 100
```

以下程式，試圖修改this本身物件的成員變數，會編譯失敗。
{% highlight c++ linenos %}
class Student{
public:
  char* name_;
  int score_;
  Student() {}
  Student(char* name, int score): name_(name), score_(score) {}
  void compare(Student* s2) const {
    this->name_ = "Alice";
    this->score_ = 100;
    if (this->score_ > s2->score_) {
      cout << "本身score比較大" << endl;
    } else {
      cout << "s2 score比較大" << endl;
    }
  }
};
int main() {
  Student s1("Mary", 80);
  Student s2("Bill", 90);
  s1.compare(&s2);
  printf("s2 name = %s, score = %d\n", s2.name_, s2.score_);
  return 0;
}
{% endhighlight %}

### const與成員變數
成員變數加上const，代表只可以在建構子中，建立物件的時候設定值，之後就不可以修改成員變數了。<br>

以下程式編譯錯誤，因為物件建立完畢，試圖修改分數`s1.score_ = 100`。
{% highlight c++ linenos %}
class Student{
public:
  char* name_;
  const int score_; //成員變數加上const
  // const成員變數，要在建構子設定初始值
  Student(char* name, int score): name_(name), score_(score) {}
};
int main() {
  // 一定要在建構子設定成員變數
  Student s1("Mary", 80);
  Student s2("Bill", 90);
  // 不可以再修改成員變數
  s1.score_ = 100;
  printf("s2 name = %s, score = %d\n", s2.name_, s2.score_);
  return 0;
}
{% endhighlight %}


[1]: {% link _pages/c/pointer/pointerConst.md %}