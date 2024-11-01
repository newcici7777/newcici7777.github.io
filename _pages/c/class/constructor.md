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

### 建立暫存物件

暫存物件(Temporary Object)建立語法
```
類別名()
Student()
```

以下的程式碼回傳暫存物件。
{% highlight c++ linenos %}
Student getStudent() {
    return Student();
}
int main() {
    Student student = getStudent();
    return 0;
}
{% endhighlight %}


#### 不要在其它有參數的建構子，呼叫暫存物件(呼叫空的建構子)

在有參數的建構子呼叫無參數的建構子，是建立暫存物件(Temporary Object)，執行完建構子，馬上執行解構子

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
        cout << "進入參數建構子" << endl;
        cout << "建立暫存物件" << endl;
        Student();
        cout << "結束暫存物件" << endl;
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, name);
        m_age = age;
    }
    ~Student() {
        cout << "解構子" << endl;
    }
    //宣告成員函式
    void print() {
        cout << "name: " << m_name << endl;
    }
};
int main() {
    Student student("Bill", 20);
    return 0;
}
{% endhighlight %}
```
進入參數建構子
建立暫存物件
沒參數建構子
解構子
結束暫存物件
解構子
```

#### 暫存物件與指標

以下程式碼建立int* m_ptr;指標，在參數為空建構子，有初始化
```
m_ptr = nullptr;
```
在解構子中，有把它釋放
```
    ~Student() {
        delete m_ptr;
    }
```

以下程式碼會執行錯誤。
原因在於暫存物件呼叫空的建構子，建立m_ptr指標，接下來執行解構子把m_ptr記憶體釋放，然後等到主程式執行完畢，要執行解構子，發現找不到m_ptr指標(因為已經被暫存物件釋放掉)。

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    int m_age = 20;
    int* m_ptr;
    Student() {
        cout << "沒參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        m_age = 0;
        m_ptr = nullptr;
    }
    Student(const char* name, const int age) {
        cout << "進入參數建構子" << endl;
        cout << "建立暫存物件" << endl;
        Student();
        cout << "結束暫存物件" << endl;
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, name);
        m_age = age;
    }
    ~Student() {
        delete m_ptr;
        cout << "解構子" << endl;
    }
    //宣告成員函式
    void print() {
        cout << "name: " << m_name << endl;
    }
};
int main() {
    Student student("Bill", 20);
    return 0;
}
{% endhighlight %}

```
進入參數建構子
建立暫存物件
沒參數建構子
解構子
結束暫存物件
malloc: *** error for object 0x1000d0010: pointer being freed was not allocated
```
## 回傳暫存物件

當暫存物件回傳給變數，這個回傳物件就不是暫存，原本在函式中的暫存物件記憶體位址就不會被釋放，反而指派至main()的接收暫存物件的變數中。

```
    Student student = Student();
    Student student1 = Student("Bill", 20);
```

可以使用以下方式建立物件

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
    ~Student() {
        cout << "解構子" << endl;
    }
};
int main() {
    Student student = Student();
    Student student1 = Student("Bill", 20);
    return 0;
}
{% endhighlight %}

```
沒參數建構子
有參數建構子
解構子
解構子
```

## 建立兩次物件

以下語法會建立兩次物件
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
沒參數建構子
沒參數建構子
解構子
解構子
```

## 多個建構子使用的程式碼寫在成員函式中

以下建構子呼叫init()函式，清空成員變數。

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