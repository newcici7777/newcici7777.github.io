---
title: 函式傳回值
date: 2025-09-08
keywords: c++, function return value 
---
## 傳回基本型態
以下是簡單傳回值的程式碼。
{% highlight c++ linenos %}
int func1() {
  int x = 10;
  return x;
}
int main() {
  int x = func1();
  cout << x << endl;
  return 0;
}
{% endhighlight %}

編譯器轉換後的程式碼:
{% highlight c++ linenos %}
void func(int* return_addr) {  // 傳入返回地址
    int x = 10;
    * return_addr = x;  // 將結果寫入返回地址
}
int main() {
    int ret;    // 傳回值暫存區
    func(&ret); // 傳遞暫存區地址    
    int y = ret; // 從暫存區賦值給y
    cout << y << endl;    
    return 0;
}
{% endhighlight %}
呼叫函式時，傳入暫存區的記憶體位址。<br>
在func()函式中，對暫存區的位址使用\*取值運算子，修改成x。<br>

## 傳回物件
{% highlight c++ linenos %}
class Student {
public:
    Student() { cout << "建構子 " << endl; }
    Student(const Student&) { cout << "拷貝" << endl; }
    ~Student() { cout << "解構子" << endl; }
};
Student getStudent() {
  Student s;
  return s;
}
int main() {
  Student s1 = getStudent();
  cout << "物件記憶體位址 = " << &s1 << endl;
  return 0;
}
{% endhighlight %}

編譯器轉換後的程式碼:
{% highlight c++ linenos %}
// 編譯器生成的隱藏結構（用於傳回值）
struct Student_ret {
    Student value;  // 傳回值暫存區
};

// 轉換後的getStudent函式
void getStudent(Student_ret* return_addr) {
    // 1. 臨時物件(呼叫建構子)
    Student temp;
    
    // 2. 拷貝到傳回值暫存區
    return_addr->value = temp;  // 呼叫拷貝
    
    // 3. 臨時物件解構
    temp.Student::~Student();
    // 函式返回，result指向的記憶體仍然有效
}

// 轉換後的main函式
int main() {
    // 傳回值暫存區
    Student_ret ret;
    
    // 呼叫函式，傳入傳回值地址
    getStudent(&ret);
    
    // 呼叫建構子
    Student s1;
    s1= ret.value;  // 呼叫拷貝
    
    // 傳回值暫存區解構
    ret.value.Student::~Student();
    
    cout << "物件記憶體位址 = " << &s1 << endl;
    
    // s1 解構
    s1.Student::~Student();
    return 0;
}
{% endhighlight %}