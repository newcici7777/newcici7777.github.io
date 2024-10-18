---
title: 深淺拷貝
date: 2024-10-18
keywords: c++, Copy constructor
---

## 淺拷貝

### 淺拷貝介紹

二個物件二個指標都指向同一個記憶體位址，一旦一個指標指向的記憶體位址釋放，使用到另一個指標就會執行錯誤，因為它指向的記憶體位址已經被釋放。

以下程式碼，在建構子時，對m_ptr指標動態分配記憶體位址，並把位址存放的值設為abcdef，在解構子中，把m_ptr指標記憶體釋放，若沒使用拷貝函式前，以下程式碼運作正常。

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    char* m_ptr;
public:
    Student() {
        m_ptr = new char[100];
        strcpy(m_ptr, "abcdef");
        cout << "沒參數建構子" << endl;
    }
    Student(const Student &s) {
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, s.m_name);
        //拷貝m_ptr
        m_ptr = s.m_ptr;
        cout << "呼叫Student(const Student &s)拷貝函式" << endl;
    }
    ~Student() {
        delete [] m_ptr;
        m_ptr = nullptr;
        cout << "解構子" << endl;
    }
    void print() {
        cout << "name: " << m_name << endl;
        //印出記憶體位址
        cout << "m_ptr address:" << &m_ptr << endl;
        cout << "m_ptr value = " << m_ptr << endl;
    }
};
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    s1.print();
    return 0;
}    
{% endhighlight %}

```
沒參數建構子
name: Cici
m_ptr address:0x7ff7bfeff460
m_ptr value = abcdef
解構子
```

### 淺拷貝刪除問題

一旦使用拷貝函式，在解構子時就會出現問題，因為s1與s2的m_ptr指向同一個記憶體位址，但s1的解構子把m_ptr記憶體釋放掉，s2的解構子就找不到m_ptr的記憶體位址。

{% highlight c++ linenos %}
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    s1.print();
    Student s2(s1);
    s2.print();
    return 0;
}    
{% endhighlight %}
```
沒參數建構子
name: Cici
m_ptr address:0x7ff7bfeff460
m_ptr value = abcdef
呼叫Student(const Student &s)拷貝函式
name: Cici
m_ptr address:0x7ff7bfeff420
m_ptr value = abcdef
解構子
lsn11(15006,0x100098600) malloc: *** error for object 0x600002900000: pointer being freed was not allocated
lsn11(15006,0x100098600) malloc: *** set a breakpoint in malloc_error_break to debug
```

### 淺拷貝修改同一個記憶體位址內容問題

以下程式碼，s1.m_ptr的指標內容為abcdef，s2.m_ptr也修改成zzzzz，造成s1.print()印出的結果也是zzzzz

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    char* m_ptr;
public:
    Student() {
        m_ptr = nullptr;
        cout << "沒參數建構子" << endl;
    }
    Student(const Student &s) {
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, s.m_name);
        //拷貝m_ptr
        m_ptr = s.m_ptr;
        cout << "呼叫Student(const Student &s)拷貝函式" << endl;
    }
    ~Student() {
        delete [] m_ptr;
        m_ptr = nullptr;
        cout << "解構子" << endl;
    }
    void print() {
        cout << "name: " << m_name << endl;
        //印出記憶體位址
        cout << "m_ptr address:" << &m_ptr << endl;
        cout << "m_ptr value = " << m_ptr << endl;
    }
};
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    s1.m_ptr = new char[100];
    strcpy(s1.m_ptr, "abcdef");
    Student s2(s1);
    strcpy(s2.m_ptr, "zzzzz");
    cout << "##### s1 #### " << endl;
    s1.print();
    cout << "##### s2 #### " << endl;
    s2.print();
    return 0;
}   
{% endhighlight %}

```
沒參數建構子
呼叫Student(const Student &s)拷貝函式
##### s1 #### 
name: Cici
m_ptr address:0x7ff7bfeff460
m_ptr value = zzzzz
##### s2 #### 
name: Cici
m_ptr address:0x7ff7bfeff420
m_ptr value = zzzzz
```

## 深拷貝

二個物件二個指標分別指向不同記憶體位址，複製的時候，把值複製到另一個記憶體位址。

以下程式碼在拷貝函式先判斷拷貝的對象是不是nullptr，若不是，才動態分配記憶體位址，並把字串指標的內容透過strcpy()拷貝到另一個字串指標。

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    char* m_ptr;
public:
    Student() {
        m_ptr = nullptr;
        cout << "沒參數建構子" << endl;
    }
    Student(const Student &s) {
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, s.m_name);
        //拷貝m_ptr
        if(s.m_ptr) {
            m_ptr = new char[100];
            strcpy(m_ptr, s.m_ptr);
        } else {
            m_ptr = nullptr;
        }
        cout << "呼叫Student(const Student &s)拷貝函式" << endl;
    }
    ~Student() {
        delete [] m_ptr;
        m_ptr = nullptr;
        cout << "解構子" << endl;
    }
    void print() {
        cout << "name: " << m_name << endl;
        //印出記憶體位址
        cout << "m_ptr address:" << &m_ptr << endl;
        cout << "m_ptr value = " << m_ptr << endl;
    }
};
{% endhighlight %}
