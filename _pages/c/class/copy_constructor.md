---
title: 拷貝函式
date: 2024-10-18
keywords: c++, Copy constructor
---

## 複製語法

已建立的物件成員變數的值，複製給另一個物件。

語法如下
```
類別 新物件變數名(已建立物件變數名);
類別 新物件變數名 = 已建立物件變數名;
```

拷貝函式程式會自動產生，會自動產生二個物件的成員變數拷貝程式碼。

## 呼叫拷貝函式

以下二種程式碼都是呼叫預設的拷貝函式

### 方式一

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
public:
    Student() {
        cout << "沒參數建構子" << endl;
    }
    ~Student() {
        cout << "解構子" << endl;
    }
    void print() {
        cout << "name: " << m_name << endl;
    }
};
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    Student s2 = s1;
    s2.print();
    return 0;
}    
{% endhighlight %}

### 方式二

{% highlight c++ linenos %}
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    Student s2(s1);
    s2.print();
    return 0;
}    
{% endhighlight %}

由執行結果可以發現建立2個物件(s1,s2)只呼叫一次拷貝函式。
s2.print()印出的值是複製s1的成員變數。

```
沒參數建構子
name: Cici
解構子
解構子
```

## 拷貝函式自行實作

```
類別名(const 類別名& 參數名) {...}
```

- 權限為public
- 函式名必須與類別名相同
- 沒有傳回值，不寫void
- 如果自行實作拷貝函式，編譯器就不會自動產生預設拷貝函式
- 如果自行實作拷貝函式，但裡面都沒有程式碼，編譯器會自動產生預設的程式碼。
- 拷貝函式參數要包含const 類別名&參考，不包含的話，就是建構子。

{% highlight c++ linenos %}
    Student(const Student &s) {
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, s.m_name);
        cout << "呼叫Student(const Student &s)拷貝函式" << endl;
    }
{% endhighlight %}

完整程式碼

{% highlight c++ linenos %}
class Student {
public:
    char m_name[50];
public:
    Student() {
        cout << "沒參數建構子" << endl;
    }
    Student(const Student &s) {
        memset(m_name,0,sizeof(m_name));
        strcpy(m_name, s.m_name);
        cout << "呼叫Student(const Student &s)拷貝函式" << endl;
    }
    ~Student() {
        cout << "解構子" << endl;
    }
    void print() {
        cout << "name: " << m_name << endl;
    }
};
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    Student s2(s1);
    s2.print();
    return 0;
}    
{% endhighlight %}

```
沒參數建構子
呼叫Student(const Student &s)拷貝函式
name: Cici
解構子
解構子
```

## 函式的參數為類別的值，會呼叫拷貝函式

{% highlight c++ linenos %}
void func(Student s) {
    s.print();
}
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    func(s1);
    return 0;
}    
{% endhighlight %}

```
沒參數建構子
呼叫Student(const Student &s)拷貝函式
name: Cici
解構子
解構子
```
## 函式傳回值為類別的值，會呼叫拷貝函式

在Linux/mac執行時不會呼叫，在windows會呼叫拷貝函式。

Linux中只呼叫一次建構子跟一次解構子，g++認為沒必要離開函式把函式建立的物件記憶體釋放，到main主程式用另一個變數指向同一個記憶體位址。

{% highlight c++ linenos %}
Student func() {
    Student s;
    strcpy(s.m_name, "Cici");
    cout << "函式物件記憶體位址 = " << &s << endl;
    return s;
}
int main() {
    Student s1 = func();
    s1.print();
    cout << "物件記憶體位址 = " << &s1 << endl;
    return 0;
}    
{% endhighlight %}  

```
沒參數建構子
函式物件記憶體位址 = 0x7ff7bfeff430
name: Cici
物件記憶體位址 = 0x7ff7bfeff430
解構子
```  

## 拷貝函式多載(Overload)

在拷貝函式增加參數str
{% highlight c++ linenos %}
    Student(const Student &s, string str) {
        memset(m_name,0,sizeof(m_name));
        string temp = str + s.m_name;
        strcpy(m_name, temp.c_str());
        cout << "呼叫Student(const Student &s, string str)拷貝函式" << endl;
    }
{% endhighlight %}  

執行的main函式

{% highlight c++ linenos %}
int main() {
    Student s1;
    strcpy(s1.m_name, "Cici");
    Student s2(s1, "漂亮的");
    s2.print();    
    return 0;
}    
{% endhighlight %} 

```
沒參數建構子
呼叫Student(const Student &s, const char str[50])拷貝函式
name: 漂亮的Cici
解構子
解構子
```   