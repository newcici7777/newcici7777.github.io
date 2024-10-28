---
title: 類別與物件
date: 2024-10-14
keywords: c++, class 
---

## 權限

類別權限預設是private，結構權限預設是public

public與private與其它權限，可以在類別中出現很多次，以下public就出現2次，private出現2次

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

## 成員屬性命名

命名方式使用`m_成員屬性`名字或`成員屬性_`

- 前綴加上m_
- 後綴加上底線_

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    int m_sex;
    enum {girl = 0, boy = 1};
private:
    char m_address[100];
public:
    int m_age = 20;
private:
    char m_father[50];
public:
    Student() {
        memset(m_name,0,sizeof(m_name));
        
    }
    void setName(const char* name) {
        strcpy(this->m_name, name);
    }
    void setAge(const int age) {
        this->m_age = age;
    }
    
    void print() {
        cout << "name: " << m_name << endl;
        cout << "age: " << m_age << endl;
        cout << "sex: ";
        if(m_sex == girl)
            cout << "girl" << endl;
        else
            cout << "boy" <<endl;
    }
};
int main() {
    Student student;
    student.setName("Bill");
    student.setAge(20);
    student.m_sex = student.girl;
    student.print();
    return 0;
}
{% endhighlight %}


## 類別作為函式參數

使用類別作為函式參數，都是使用參考的方式傳遞。

string是類別，作為函式參數，使用參考的方式傳遞。

{% highlight c++ linenos %}
void func1(const string& msg) {
    cout << msg << endl;
}
int main() {
    func1("test");
    return 0;
}
{% endhighlight %}

```
test
```

## 類別中的函式自動變為內嵌函式(inline)

- [inline][1]

類別中的函式自動變為內嵌函式，但不是在類別中的函式不會變成內嵌函式。

以下print()是內嵌函式

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    void print() {
        cout << "name: " << m_name << endl;
        cout << "age: " << m_age << endl;
        cout << "sex: ";
        if(m_sex == girl)
            cout << "girl" << endl;
        else
            cout << "boy" <<endl;
    }
};
{% endhighlight %}

print()成員函式程式碼在類別之外定義，定義方式如下。

```
回傳值 類別名::函式名(){程式碼}
```

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
    void print();
};
void Student::print() {
    cout << "test" << endl;
}
int main() {
    Student student;
    student.print();
    return 0;
}
{% endhighlight %}


[1]: {% link _pages/c/reference/function.md %}#內嵌函式inline
