---
title: 函式參數為物件
date: 2024-10-14
keywords: c++, pointer pass to a function
---
以下為傳參考與傳指標的方式。<br>
但是類別本身是值，不是記憶體位址，所以傳入的參數為指標，要加上`&`物件。<br>
若傳入的參數為陣列，陣列名預設為記憶體位址，所以傳入函式「不用」加`&`陣列名。<br>
{% highlight c++ linenos %}
class Student {
 public:
  const char* name_;
  int id_;
};
void callRef(const Student& s) {
  cout << "ref = " << s.name_ << endl;
}
void callPoint(Student* s) {
  cout << "point = " << s->name_ << endl;
}
int main() {
  Student s;
  s.name_ = "Bill";
  callRef(s);
  callPoint(&s);
  return 0;
}
{% endhighlight %}
```
ref = Bill
point = Bill
```