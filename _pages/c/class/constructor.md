---
title: 建構子與解構子
date: 2024-10-16
keywords: c++, constructor 
---

## 建構子

語法
```
public:
類別名() {
}
Student(){
}
```

- 與類別的名字相同
- 沒有返回值
- 權限是public
- 可以有參數，參數也可以有預設值

### 參數為空的建構子

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
public:
    Student() {
        cout << "沒參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
    }
};
int main() {
    Student student;
    return 0;
}  
{% endhighlight %}
```
沒參數建構子
```
### 建構子參數

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    int m_age = 20;
    Student() {
        cout << "沒參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        m_age = 0;
    }
    Student(const char* name, const int age) {
        cout << "有參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, name);
        m_age = age;
    }
    //宣告成員函式
    void print() {
        cout << "name: " << m_name << endl;
    }
};
int main() {
    Student student;
    Student student1("Bill", 20);
    student1.print();
    return 0;
}
{% endhighlight %}

```
沒參數建構子
有參數建構子
name: Bill
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
class Student {
public:
    char m_name[50];
public:
    Student() {
        cout << "建構子" << endl;
        memset(m_name,0,sizeof(m_name));
    }
    ~Student() {
        cout << "解構子" << endl;
    }
};
int main() {
    Student student;
    return 0;
}
{% endhighlight %}

```
建構子
解構子
```

## 建構子參數只有一個，可使用指派運算子

建構子參數只有一個，可使用指派運算子=，呼叫只有一個參數的建構子

{% highlight c++ linenos %}
class Student {
public:
    string name;
    Student(){};
    //參數只有一個建構子
    Student(const char* name) {
        this->name = name;
    }
};
int main() {
    //使用等於(=)指派運算子呼叫只有一個參數的建構子
    Student student = "Bill";
    cout << "name = " << student.name << endl;
    return 0;
}
{% endhighlight %}
```
name = Bill
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
class Student {
public:
    char m_name[50];
    int m_age = 20;
    Student(const char* name, const int age) {
        cout << "有參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, name);
        m_age = age;
    }
    //宣告成員函式
    void print() {
        cout << "name: " << m_name << endl;
    }
};
int main() {
    Student student;
    return 0;
} 
{% endhighlight %}

### 不要用變數名()建立物件

以下程式碼，會編譯成功，但不會呼叫建構子，建立物件失敗，編譯器認為是呼叫函式名為student()的函式。

注意！student()是小寫，是變數名，不是類別名Student

```
    Student student();
```
執行結果為空，沒有印出"沒參數建構子"

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    int m_age = 20;
    Student() {
        cout << "沒參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        m_age = 0;
    }
    Student(const char* name, const int age) {
        cout << "有參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, name);
        m_age = age;
    }
    //宣告成員函式
    void print() {
        cout << "name: " << m_name << endl;
    }
};
int main() {
    Student student();
    return 0;
}
{% endhighlight %}

用以上student呼叫成員函式print()，會編譯失敗，因為根本沒有建立物件，沒有在記憶體產生物件存放的位址。

```
student.print();
```

{% highlight c++ linenos %}
int main() {
    Student student();
    student.print();
    return 0;
} 
{% endhighlight %}

## 多個建構子使用的程式碼寫在成員函式中

以下建構子呼叫init()函式，清空成員變數，多個建構子可以使用。

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    int m_age = 20;
    Student() {
        cout << "沒參數建構子" << endl;
        init();
    }
    Student(const char* name, const int age) {
        cout << "有參數建構子" << endl;
        init();
        strcpy(m_name, name);
        m_age = age;
    }
    ~Student() {
        cout << "解構子" << endl;
    }
    void init() {
        memset(m_name,0,sizeof(m_name));
        m_age = 0;
    }
};
int main() {
    Student student = Student();
    Student student1 = Student("Bill", 20);
    return 0;
}
{% endhighlight %}


