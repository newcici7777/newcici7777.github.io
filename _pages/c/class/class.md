---
title: 類別與物件
date: 2024-10-14
keywords: c++, class 
---
## 成員變數權限
類別成員變數權限預設是private，結構成員變數權限預設是public。<br>

public與private與其它權限，可以在類別中出現很多次，以下public就出現2次，private出現2次。<br>

{% highlight c++ linenos %}
class Student {
public:
  char name[50];
private:
  char address[100];
public:
  int age;
private:
  char father[50];
};
{% endhighlight %}

## 成員變數命名
命名方式使用`成員變數_`，成員變數名 \+ 加上底線_。<br>
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s;
  s.name_ = "Bill";
  return 0;
}
{% endhighlight %}

## 成員變數初始化
在[變數預設值][4]與[函式區域變數初始值][3]文章中提過，區域變數不會有預設值，所以印出區域變數都是垃圾資料。<br>
{% highlight c++ linenos %}
int main() {
  int n;
  cout << n << endl;
  return 0;
}
{% endhighlight %}
```
15824
```

相同的，物件的成員變數也不會有初始值，所以印出來都是垃圾資料。<br>
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s;
  cout << s.name_ << endl;
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
H\211\330H\203\304[]\303UH\211\345H\205\366tTE1\311A\2700
-1074793600
```

基本型態與指標預設值如下:

|型態|預設值|
|int|0|
|char|`\0`|
|float|0.0|
|double|0.0|
|指標類型|NULL|

使用以下方法，成員變數就會設為預設值。

### 類別()
類別()會把成員變數初始化為預設值，注意！<span class="markline">不能有自訂建構子()</span>，有自訂建構子()，印出來仍是垃圾資料。<br>
```
類別 物件變數 = 類別()
Student s = Student()
```

無建構子程式碼。
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s = Student();
  if (s.name_ == nullptr) {
      cout << "name_ is nullptr" << endl;
  } else {
      cout << "name_: " << s.name_ << endl;
  }
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
name_ is nullptr
0
```

有建構子程式碼:
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
  Student() {}
};
int main() {
  Student s = Student();
  if (s.name_ == nullptr) {
      cout << "name_ is nullptr" << endl;
  } else {
      cout << "name_: " << s.name_ << endl;
  }
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
name_: H\211\330H\203\304[]\303UH\211\345H\205\366tTE1\311A\2700
-1074793600
```

### 物件變數{}
物件變數{}會把成員變數初始化為預設值，注意！<span class="markline">不能有自訂建構子()</span>，有自訂建構子()，印出來仍是垃圾資料。<br>
```
類別 物件變數{}
Student s{}
```

無建構子程式碼。
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
int main() {
  Student s{};
  if (s.name_ == nullptr) {
      cout << "name_ is nullptr" << endl;
  } else {
      cout << "name_: " << s.name_ << endl;
  }
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
name_ is nullptr
0
```

有建構子程式碼:
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
  Student() {}
};
int main() {
  Student s{};
  if (s.name_ == nullptr) {
      cout << "name_ is nullptr" << endl;
  } else {
      cout << "name_: " << s.name_ << endl;
  }
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
name_: H\211\330H\203\304[]\303UH\211\345H\205\366tTE1\311A\2700
-1074793600
```

### 成員變數初始化
指標變數，可以設nullptr;
{% highlight c++ linenos %}
class Student {
 public:
  // 成員變數初始化
  const char* name_ = nullptr;
  int id_ = 0;
  Student() {}
};
int main() {
  Student s;
  if (s.name_ == nullptr) {
      cout << "name_ is nullptr" << endl;
  } else {
      cout << "name_: " << s.name_ << endl;
  }
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
name_ is nullptr
0
```
### 建構子初始化
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
  Student() {
    name_ = nullptr;
    id_ = 0;
  }
};
int main() {
  Student s;
  if (s.name_ == nullptr) {
      cout << "name_ is nullptr" << endl;
  } else {
      cout << "name_: " << s.name_ << endl;
  }
  cout << s.id_ << endl;
  return 0;
}
{% endhighlight %}
```
name_ is nullptr
0
```
### 建構子初始化2
使用以下方式初始化。
```
建構子() : 成員變數名(初始值), 成員變數名(初始值) {}
Student() : name_(nullptr), id_(0) {}
```

程式碼:
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
  Student() : name_(nullptr), id_(0) {
  }
};
{% endhighlight %}
```
name_ is nullptr
0
```

## 類別作為函式參數
以下為傳參考與傳指標的方式。<br>
陣列名預設為記憶體位址，所以傳入函式「不用」加`&`陣列名。<br>
但是類別本身是值，不是記憶體位址，所以傳入的參數為指標，要加上`&`物件。<br>
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
void callRef(const Student& s) {
  cout << "ref = " << s.name_ << endl;
}
void callPoint(Student* s) {
  cout << "point = " << s->name_ << endl;
}
int main() {
  Student s;
  s.name_ = "Bill";
  callRef(s);
  callPoint(&s);
  return 0;
}
{% endhighlight %}
```
ref = Bill
point = Bill
```

## 成員函式在類別外定義

- [函式宣告與定義][2]

print()成員函式程式碼在類別之外定義，定義方式如下。

```
傳回值 類別名::函式名(){程式碼}
```

{% highlight c++ linenos %}
class Student {
public:
  char name_[50];
  //宣告函式
  void print();
};
//類別外部定義
void Student::print() {
  cout << "test" << endl;
}
int main() {
  Student student;
  student.print();
  return 0;
}
{% endhighlight %}
```
test
```

[1]: {% link _pages/c/function/func_inline.md %}
[2]: {% link _pages/c/function/func_def.md %}
[3]: {% link _pages/c/function/callByValue.md %}#區域變數初始值
[4]: {% link _pages/c/basic/scope.md %}#預設值
