---
title: 泛型
date: 2026-03-31
keywords: flutter, dart, generics
---
泛型:限定傳入的類型要跟跟尖括號定義的一樣。<br>

語法:
```
<T>
```

## 方法泛型
![img]({{site.imgurl}}/flutter/generics1.png)<br>

以下程式碼請比對上圖。<br>
{% highlight dart linenos %}
void main() {
  bool res = getValue<bool>(true);
  int res2 = getValue<int>(10);
  String res3 = getValue<String>("hello");
}

T getValue<T>(T value) {
  return value;+
}
{% endhighlight %}

使用`<T>`來限制傳入參數類型。

## 泛型類型

![img]({{site.imgurl}}/flutter/generics2.png)<br>

限定建構子傳入的類型要跟跟尖括號定義的一樣。
{% highlight dart linenos %}
void main() {
  Student<String, int> stu1 = Student(name: "Bill", age: 1);
  print(stu1.name);
  print(stu1.age);
}

class Student<T1, T2> {
  T1? name;
  T2? age;
  Student({this.name, this.age});
}
{% endhighlight %}