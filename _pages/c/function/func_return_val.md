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

編譯器轉換後的程式碼:main的部分<br>
1. main()函式會建立一個傳回值暫存區的物件。
2. 把暫存區的位址傳入getStudent()函式中。

getStudent()函式部分:<br>
1. 在getStudent()函式中，建立temp物件。
2. 對暫存區指標使用\*取值運算子，取出物件，把temp物件拷貝過去。
3. 執行完getStudent()函式，temp會自動呼叫解構子，回收記憶體位址。

main()函式部分:<br>
1. 建立s1物件。
2. 將「傳回值」暫存區的物件拷貝到s1中。

{% highlight c++ linenos %}
// 轉換後的getStudent函式
void getStudent(Student* ret) {
    // 1. 建立臨時物件
    Student temp;
    
    // 2. 拷貝到傳回值暫存區
    *ret = temp;  // 呼叫拷貝
    
    // 3. 臨時物件解構
    // 會自動呼叫解構子，不用手動呼叫
    //temp.~Student();
    // 函式返回，return_addr指向的記憶體仍然有效
}

// 轉換後的main函式
int main() {
    // 傳回值暫存區
    Student ret;
    
    // 呼叫函式，傳入傳回值地址
    getStudent(&ret);
    
    // 建立s1物件，呼叫建構子
    Student s1;
    s1 = ret;  // 呼叫拷貝
    
    // 傳回值暫存區解構
    // 會自動呼叫解構子，不用手動呼叫
    //ret.~Student();
    
    cout << "物件記憶體位址 = " << &s1 << endl;
    
    // s1 解構
    // 會自動呼叫解構子，不用手動呼叫
    //s1.~Student();
    return 0;
}
{% endhighlight %}