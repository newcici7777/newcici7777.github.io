---
title: 建構子與解構子
date: 2024-10-16
keywords: c++, constructor 
---
Prerequisites:
- [RVO][1]

## 建構子
語法
```
public:
類別名() {
}
```

- 與類別的名字相同
- 沒有返回值
- 權限是public
- 可以有參數，參數也可以有預設值

### 參數為空的建構子
{% highlight c++ linenos %}
#include <iostream>
#include <string>
using namespace std;
class Student {
 public:
  const char* name_;
  Student() {
    cout << "無參數建構子被呼叫：物件已初始化" << endl;
  }
};
int main() {
  Student student;
  return 0;
}
{% endhighlight %}
```
無參數建構子被呼叫：物件已初始化
```

## 解構子
物件記憶體釋放前，會執行解構子。

語法 
```
public:
~類別名() {
}
~Student(){
}
```
- 與類別的名字相同，前面加上~
- 沒有返回值
- 權限是public

{% highlight c++ linenos %}
#include <iostream>
#include <string>
using namespace std;
class Student {
 public:
  const char* name_;
  Student() {
    cout << "無參數建構子被呼叫：物件已初始化" << endl;
  }
  ~Student() {
    cout << "呼叫解構子" << endl;
  }
};
int main() {
  Student student;
  return 0;
}
{% endhighlight %}
```
無參數建構子被呼叫：物件已初始化
呼叫解構子
```

## 呼叫有參數的建構子
語法:
```
類別 物件名(參數1)
Student student("Mary");
```
{% highlight c++ linenos %}
#include <iostream>
#include <string>
using namespace std;
class Student {
 public:
  const char* name_;
  Student() {
    cout << "無參數建構子被呼叫：物件已初始化" << endl;
  }
  Student(const char* name) {
    name_ = name;
    cout << "一個參數建構子 name_ = " << name_ << endl;
  }
};
int main() {
  Student student("Mary");
  return 0;
}
{% endhighlight %}
```
一個參數建構子 name_ = Mary
```

## 建構子參數只有一個，可使用指派運算子
建構子參數只有一個，可使用指派運算子=，呼叫只有一個參數的建構子<br>
```
類別 物件名 = 參數
Student student = "Mary";
```

{% highlight c++ linenos %}
#include <iostream>
#include <string>
using namespace std;
class Student {
 public:
  const char* name_;
  Student() {
    cout << "無參數建構子被呼叫：物件已初始化" << endl;
  }
  Student(const char* name) {
    name_ = name;
    cout << "一個參數建構子 name_ = " << name_ << endl;
  }
};
int main() {
  Student student= "Mary";
  return 0;
}
{% endhighlight %}
```
一個參數建構子 name_ = Mary
```

## 注意事項

### 建構子解構子自動生成

若沒實作建構子/解構子，編譯器會自動生成空的建構子與解構子，若實作建構子，不管建構子有沒有參數，編譯器不會自動生成空的建構子或解構子

如果沒有實作空的建構子，只實作有參數的建構子，以下程式碼編譯不過。

以下程式碼在主程式main函式，會尋找空的建構子。
```
Student student;
```

以下程式碼編譯不過
{% highlight c++ linenos %}
#include <iostream>
#include <string>
using namespace std;
class Student {
 public:
  const char* name_;
  Student(const char* name) {
    name_ = name;
    cout << "一個參數建構子 name_ = " << name_ << endl;
  }
};
int main() {
  Student student;
  return 0;
}
{% endhighlight %}

### 不要用變數名()建立物件
以下程式碼，會編譯成功，但不會呼叫建構子，建立物件失敗。<br>

編譯器認為是呼叫函式名為student()的函式，回傳值類型是Student。
```
Student student();
```
執行結果為空，沒有印出"無參數建構子被呼叫：物件已初始化"

{% highlight c++ linenos %}
#include <iostream>
#include <string>
using namespace std;
class Student {
 public:
  const char* name_;
  Student() {
    cout << "無參數建構子被呼叫：物件已初始化" << endl;
  }
};
int main() {
  Student student();
  return 0;
}
{% endhighlight %}

## 建構子初始化
使用建構子初始化成員變數。<br>
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

## 使用init()初始化成員變數
以下每個建構子都呼叫init()初始化成員變數，若有多個成員變數要初始化，可以全寫在init()中。<br>
{% highlight c++ linenos %}
#include <iostream>
#include <string>
using namespace std;
class Student {
 public:
  const char* name_;
  Student() {
    init();
    cout << "無參數建構子被呼叫：物件已初始化" << endl;
  }
  Student(const char* name) {
    init();
    name_ = name;
    cout << "一個參數建構子 name_ = " << name_ << endl;
  }
  void init() {
    name_ = nullptr;
  }
};
int main() {
  Student student= "Mary";
  return 0;
}
{% endhighlight %}
```
一個參數建構子 name_ = Mary
```

[1]: {% link _pages/c/editor/rvo.md %}
