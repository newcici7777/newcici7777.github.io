---
title: 臨時物件temporary object
date: 2024-11-13
keywords: c++, temporary object 
---

Prerequisites:

- [關閉RVO][1]
- [拷貝函式][2]
- [operator=()][3]
- [匿名物件][4]

把RVO關閉後，才能進行以下的範例。

## 傳回臨時物件

### 傳回臨時物件1

以下語法會觸發拷貝函式

Student()返回的臨時 Student 物件再次被拷貝給 s1

```
Student s1 = Student();
```

完整程式碼
{% highlight c++ linenos %}
class Student {
public:
  //建構子
  Student() {
    cout << "建構子" << endl;
  }
  //拷貝函式
  Student(const Student &s) {
    cout << "呼叫Student(const Student &s)拷貝函式" << endl;
  }
  //指派運算子
  Student& operator=(const Student& s) {
    cout << "呼叫Student operator=指派運算子" << endl;
    return *this;
  }
  //解構子
  ~Student() {
    cout << "解構子" << endl;
  }
};
int main() {
  Student s1 = Student();
  cout << "物件記憶體位址 = " << &s1 << endl;
  return 0;
  //離開主程式針對s1呼叫解構子
}
{% endhighlight %}

```
建構子
呼叫Student(const Student &s)拷貝函式
解構子
物件記憶體位址 = 0x7ff7bfeff468
解構子
```

### 傳回臨時物件2

函式傳回值是臨時物件會呼叫拷貝函式。

以下程式碼會呼叫二次拷貝函式。

- 第一個拷貝操作：在 return Student(); 創建的臨時 Student 物件被拷貝一次，以便返回到 main 函數。這觸發了第一次拷貝建構子呼叫。

- 第二個拷貝操作：從 getStudent() 返回的臨時 Student 物件再次被拷貝給 s1，這會觸發第二次拷貝建構子的呼叫。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  Student() {
    cout << "沒參數建構子" << endl;
    cout << "address = " << this << endl;
  }
  Student(const Student &s) {
    cout << "呼叫Student(const Student &s)拷貝函式" << endl;
  }
  Student& operator=(const Student& s) {
    cout << "呼叫Student operator=指派運算子" << endl;
    return *this;
  }
  ~Student() {
    cout << "解構子" << endl;
  }
};
Student getStudent() {
  return Student();
}
int main() {
  Student s1 = getStudent();
  cout << "物件記憶體位址 = " << &s1 << endl;
  return 0;
}
{% endhighlight %}
```
沒參數建構子
address = 0x7ff7bfeff410
呼叫Student(const Student &s)拷貝函式
解構子
呼叫Student(const Student &s)拷貝函式
解構子
物件記憶體位址 = 0x7ff7bfeff468
解構子
```

### 傳回臨時物件3

函式傳回值是臨時物件會呼叫拷貝函式。

以下程式碼會呼叫二次拷貝函式。

- 第一個拷貝操作：在 return s; 創建的臨時 Student 物件被拷貝一次，以便返回到 main 函數。這觸發了第一次拷貝建構子呼叫。

- 第二個拷貝操作：從 func() 返回的臨時 Student 物件再次被拷貝給 s1，這會觸發第二次拷貝建構子的呼叫。

{% highlight c++ linenos %}
Student func() {
  Student s;
  return s;//呼叫拷貝函式
  //離開函式，針對物件s呼叫解構子
}
{% endhighlight %}

以下的程式碼回傳臨時物件。
{% highlight c++ linenos %}
class Student {
public:
	//建構子
  Student() {
    cout << "建構子" << endl;
  }
  //拷貝函式
  Student(const Student &s) {
    cout << "呼叫Student(const Student &s)拷貝函式" << endl;
  }
  //指派運算子
  Student& operator=(const Student& s) {
    cout << "呼叫Student operator=指派運算子" << endl;
    return *this;
  }
  //解構子
  ~Student() {
    cout << "解構子" << endl;
  }
};
Student func() {
  Student s;//呼叫建構子
  cout << "函式物件記憶體位址 = " << &s << endl;
  return s;//呼叫拷貝函式
  //離開函式，針對物件s呼叫解構子
}
int main() {
  Student s1 = func();
  cout << "物件記憶體位址 = " << &s1 << endl;
  return 0;
  //離開主程式針對s1呼叫解構子
}
{% endhighlight %}
```
建構子
函式物件記憶體位址 = 0x7ff7bfeff410
呼叫Student(const Student &s)拷貝函式
解構子
呼叫Student(const Student &s)拷貝函式
解構子
物件記憶體位址 = 0x7ff7bfeff468
解構子
```
### 傳回臨時物件4

因為student是已經存在的物件，因此會呼叫指派運算子=operator()

- Student student; 這行程式碼創建了一個 Student 類別的實例，並呼叫了預設建構子。此時 student 變數已經存在。
- student = Student(); 這行程式碼產生了一個新的臨時 Student 對象，然後將它賦值給已有的 student 對象。這樣的操作會呼叫指派運算子（operator=），而不是拷貝建構子，因為 student 已經存在，不需要創建新的物件。

{% highlight c++ linenos %}
int main() {
  Student student;
  student = Student();
  return 0;
}
{% endhighlight %}


{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  //建構子
  Student() {
    cout << "建構子" << endl;
  }
  //拷貝函式
  Student(const Student &s) {
    cout << "呼叫Student(const Student &s)拷貝函式" << endl;
  }
  //指派運算子
  Student& operator=(const Student& s) {
    cout << "呼叫Student operator=指派運算子" << endl;
    return *this;
  }
  //解構子
  ~Student() {
    cout << "解構子" << endl;
  }
};
int main() {
  Student student;
  student = Student();
  return 0;
}
{% endhighlight %}
```
建構子
建構子
呼叫Student operator=指派運算子
解構子
解構子
```

## 建立臨時物件

在成員函式或其它有參數的建構子呼叫參數為空建構子，只是建立臨時物件，然後很快的又被記憶體釋放。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  //建構子
  Student() {
    cout << "建構子" << endl;
  }
  //拷貝函式
  Student(const Student &s) {
    cout << "呼叫Student(const Student &s)拷貝函式" << endl;
  }
  //指派運算子
  Student& operator=(const Student& s) {
    cout << "呼叫Student operator=指派運算子" << endl;
    return *this;
  }
  //解構子
  ~Student() {
    cout << "解構子" << endl;
  }
  void print() {
    cout << "建立臨時物件" << endl;
    Student();
    cout << "結束臨時物件" << endl;
  }
};
int main() {
  Student s1;
  s1.print();
  return 0;
  //離開主程式針對s1呼叫解構子
}
{% endhighlight %}

```
建構子
建立臨時物件
建構子
解構子
結束臨時物件
解構子
```


[1]: {% link _pages/c/editor/rvo.md %}
[2]: {% link _pages/c/class/copy_constructor.md %}
[3]: {% link _pages/c/class/operator_assign.md %}
[4]: {% link _pages/c/class/anonymous_obj.md %}