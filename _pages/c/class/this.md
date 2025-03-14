---
title: this
date: 2025-03-15
keywords: c++, this
---

## this是記憶體位址
在類別的每一個成員函式，都有一個隱藏參數為this指標，this指標為「呼叫物件」的記憶體位址。  

什麼是「呼叫物件」，以下例的程式範例來說，建立s1物件`Student s1;`，s1物件呼叫getAddressByPoint()成員函式，那麼函式中的隱藏參數this指標就是s1的記憶體位址，s1就是「呼叫物件」，也就是誰呼叫了成員函式getAddressByPoint()，this指標就是誰。

this記憶體位址存放的值就是呼叫函式的物件，也就是s1。

指標就是記憶體位址，所以函式傳回值為指標，就可以直接傳回this。

若函式傳回值為參考，就要把this指標，透過\*取值運算子，取出物件，並將物件傳回。

{% highlight c++ linenos %}
class Student{
 public:
  // 傳回值為指標
  Student* getAddressByPoint(){
    return this;
  }
  // 傳回值為參考
  Student& getAddressByRef(){
    // 透過取值運算子，取出物件
    return *this;
  }
  void func() {
    cout << "call func()" << endl;
  }
};
int main() {
  Student s1;
  Student* ptr = s1.getAddressByPoint();
  cout << "address = " << ptr << endl;
  ptr->func();
  Student& ref = s1.getAddressByRef();
  ref.func();
  return 0;
}
{% endhighlight %}
```
address = 0x7ff7bfeff2e8
call func()
call func()
```

## this指標取得成員變數

使用this指標取得成員變數要用->(箭頭)取得，因為this是指標，指向物件存放的位址。
{% highlight c++ linenos %}
class Student{
 public:
  string name;
  Student* getAddressByPoint(){
    return this;
  }
  Student& getAddressByRef(){
    return *this;
  }
  void func() {
    cout << "call func() name = " << this->name << endl;
  }
};
int main() {
  Student s1;
  s1.name = "Cici";
  Student* ptr = s1.getAddressByPoint();
  cout << "address = " << ptr << endl;
  ptr->func();
  Student& ref = s1.getAddressByRef();
  ref.func();
  return 0;
}
{% endhighlight %}
```
address = 0x7ff7bfeff2d0
call func() name = Cici
call func() name = Cici
```

## this比較
程式碼中的this是指呼叫函式的物件。  
`s1.compare(s2)`這段程式碼而言，this就是s1。  
{% highlight c++ linenos %}
class Student{
public:
    Student();
    ~Student();
    Student* getAddress(){
        return this;
    }
    int getWeight() {
        return this->weight;
    }
    void setWeight(int weight) {
        this->weight = weight;
    }
    int compare(Student& s) {
        return this->weight > s.weight;
    }
private:
    int weight;
};
int main() {
    Student s1;
    s1.setWeight(50);
    Student s2;
    s2.setWeight(45);
    if(s1.compare(s2)) {
        cout << "s1比較重 " << endl;
    } else {
        cout << "s2比較重 " << endl;
    }
    return;
}
{% endhighlight %}