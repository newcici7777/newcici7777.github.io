---
title: cast operator型別轉換運算子
date: 2025-02-25
keywords: c++, cast operator
---
Prerequisites:
- [建構子轉型][1]

在建構子轉型的文章中，是把string轉成類別、int轉成類別、double轉成類別

cast operator，把類別轉換成某種類型，比如類別轉成int、類別轉成string、類別轉成float

語法，必須放在類別中，沒有傳回值類型，不能有參數。
```
operator 類型()
```

{% highlight c++ linenos %}
int main() {
  Student student;
  student.name_ = "Bill";
  student.age_ = 20;
  student.weight_ = 50;
  // 自動轉型
  string name = student;
  int age = student;
  double weight = student;
  cout << "name = " << name << endl;
  cout << "age = " << age << endl;
  cout << "weight = " << weight << endl;
  // 強制轉換
  string name2 = (string)student;
  int age2 = (int)student;
  double weight2 = (double)student;
  cout << "name2 = " << name2 << endl;
  cout << "age2 = " << age2 << endl;
  cout << "weight2 = " << weight2 << endl;
  string name3 = string(student);
  int age3 = int(student);
  double weight3 = double(student);
  cout << "name3 = " << name3 << endl;
  cout << "age3 = " << age3 << endl;
  cout << "weight3 = " << weight3 << endl;
  return 0;
}
{% endhighlight %}

```
name = Bill
age = 20
weight = 50
name2 = Bill
age2 = 20
weight2 = 50
name3 = Bill
age3 = 20
weight3 = 50
```

[1]: {% link _pages/c/conversion/conversion_function.md %}
