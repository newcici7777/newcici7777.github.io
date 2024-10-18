---
title: 建構子與解構子
date: 2024-10-16
keywords: c++, constructor 
---

## 建構子

### 初始化

主要把成員變數初始化

- 與類別的名字相同
- 沒有返回值
- 權限是public
- 可以有參數，參數也可以有預設值

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
private:
    char m_address[100];
public:
    int m_age = 20;
private:
    char m_father[50];
public:
    Student() {
    	cout << "沒參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        memset(m_address,0,sizeof(m_address));
        memset(m_father,0,sizeof(m_address));
        m_age = 0;
    }
};
int main() {
    Student student;
    return 0;
}    
{% endhighlight %}

### 建構子參數

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
private:
    char m_address[100];
public:
    int m_age = 20;
private:
    char m_father[50];
public:
    Student() {
        cout << "沒參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        memset(m_address,0,sizeof(m_name));
        memset(m_father,0,sizeof(m_name));
        m_age = 0;
    }
    Student(const char* name, const char* address, const char* father) {
        cout << "有參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        memset(m_address,0,sizeof(m_name));
        memset(m_father,0,sizeof(m_name));
        strcpy(m_name, name);
        strcpy(m_address, address);
        strcpy(m_father, father);
    }
    void setName(const char* name) {
        strcpy(this->m_name, name);
    }
    void setAge(const int age) {
        this->m_age = age;
    }
    //宣告成員函式
    void print();
};
//定義成員函式(寫程式碼)
void Student::print() {
    cout << "name: " << m_name << endl;
    cout << "address: " << m_address << endl;
    cout << "father: " << m_father << endl;
}
int main() {
    Student student;
    Student student1("bill","桃園蘆竹","abcd");
    student1.print();
    return 0;
} 
{% endhighlight %}

```
沒參數建構子
有參數建構子
name: bill
address: 桃園蘆竹
father: abcd
```
## 注意事項

### 建構子解構子自動生成

若沒實作建構子/解構子，編譯器會自動生成空的建構子與解構子，若實作建構子，不管建構子有沒有參數，編譯器不會自動生成空的建構子或解構子

如果沒有實作空的建構子，只實作有參數的建構子，以下程式碼編譯不過。

以下程式碼在主程式main函式，會尋找空的建構子。

```
Student student;
```

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
private:
    char m_address[100];
public:
    int m_age = 20;
private:
    char m_father[50];
public:
    Student(const char* name, const char* address, const char* father) {
        cout << "有參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        memset(m_address,0,sizeof(m_name));
        memset(m_father,0,sizeof(m_name));
        strcpy(m_name, name);
        strcpy(m_address, address);
        strcpy(m_father, father);
    }
    void setName(const char* name) {
        strcpy(this->m_name, name);
    }
    void setAge(const int age) {
        this->m_age = age;
    }
    //宣告成員函式
    void print();
};
//定義成員函式(寫程式碼)
void Student::print() {
    cout << "name: " << m_name << endl;
    cout << "address: " << m_address << endl;
    cout << "father: " << m_father << endl;
}
int main() {
    Student student;
    return 0;
} 
{% endhighlight %}

### 不要用圓括號()建立物件

以下程式碼，會編譯成功，但不會呼叫建構子，建立物件失敗
```
    Student student();
```

用以上student呼叫成員函式，會編譯失敗，因為根本沒有建立物件，沒有在記憶體產生物件存放的位址。

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

### 不要在其它有參數的建構子，呼叫空的建構子

在有參數的建構子呼叫無參數的建構子，是建立暫存物件(Temporary Object)，執行完建構子，馬上執行解構子

{% highlight c++ linenos %}
Student(const char* name, const char* address, const char* father) {
    cout << "建立暫存物件" << endl;
    Student();
    cout << "結束暫存物件" << endl;
}
{% endhighlight %}

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
原因在於臨時物件呼叫空的建構子，建立m_ptr指標，接下來執行解構子把m_ptr記憶體釋放，然後等到主程式執行完畢，要執行解構子，發現找不到m_ptr指標(因為已經被臨時物件釋放掉)。

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    int* m_ptr;
private:
    char m_address[100];
public:
    int m_age = 20;
private:
    char m_father[50];
public:
    Student() {
        cout << "沒參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        memset(m_address,0,sizeof(m_name));
        memset(m_father,0,sizeof(m_name));
        m_age = 0;
        m_ptr = nullptr;
    }
    Student(const char* name, const char* address, const char* father) {
        cout << "建立暫存物件" << endl;
        Student();
        cout << "結束暫存物件" << endl;
        cout << "有參數建構子" << endl;
        memset(m_name,0,sizeof(m_name));
        memset(m_address,0,sizeof(m_name));
        memset(m_father,0,sizeof(m_name));
        strcpy(m_name, name);
        strcpy(m_address, address);
        strcpy(m_father, father);
    }
    ~Student() {
        delete m_ptr;
    }
    void setName(const char* name) {
        strcpy(this->m_name, name);
    }
    void setAge(const int age) {
        this->m_age = age;
    }
    void print();
};
void Student::print() {
    cout << "name: " << m_name << endl;
    cout << "address: " << m_address << endl;
    cout << "father: " << m_father << endl;
}
int main() {
    Student student1("bill","桃園蘆竹","abcd");
    return 0;
} 
{% endhighlight %}

```
建立暫存物件
沒參數建構子
結束暫存物件
有參數建構子
malloc: *** error for object 0x1000d0010: pointer being freed was not allocated
```
## 匿名方式建立物件

可以使用以下方式建立物件
{% highlight c++ linenos %}
    Student student = Student();
    Student student1 = Student("bill","桃園蘆竹","abcd");
{% endhighlight %}


## 建立二次物件

以下語法會建立二次物件
{% highlight c++ linenos %}
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
    int* m_ptr;
private:
    char m_address[100];
public:
    int m_age = 20;
private:
    char m_father[50];
public:
    Student() {
        cout << "沒參數建構子" << endl;
        init();
        m_age = 0;
        m_ptr = nullptr;
    }
    Student(const char* name, const char* address, const char* father) {
        cout << "有參數建構子" << endl;
        init();
        strcpy(m_name, name);
        strcpy(m_address, address);
        strcpy(m_father, father);
        m_ptr = nullptr;
    }
    ~Student() {
        delete m_ptr;
        cout << "解構子" << endl;
    }
    void setName(const char* name) {
        strcpy(this->m_name, name);
    }
    void setAge(const int age) {
        this->m_age = age;
    }
    void print();
private:
    void init() {
        memset(m_name,0,sizeof(m_name));
        memset(m_address,0,sizeof(m_name));
        memset(m_father,0,sizeof(m_name));
    }
};
{% endhighlight %}