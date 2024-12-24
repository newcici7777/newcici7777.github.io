---
title: 靜態類別變數與函式
date: 2024-10-27
keywords: c++, Static Class Data 
---

Prerequisites:

[記憶體配置][1]

靜態類別變數與靜態類別函式不屬於物件，與物件是分開，靜態類別變數與靜態類別函式存放在靜態bss與data segment

靜態類別變數只有一個，無論物件有多少個，但靜態類別變數只有一個。

## 靜態類別變數

在變數前面加上static 

語法
```
static int count;
```

靜態類別變數不會在建立物件的時候初始化，需要使用::範圍運算子在全域變數的位置初始化。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
  static int count;
};
//在這邊初始化靜態類別變數
int Student::count = 100;
int main() {
  return 0;
}
{% endhighlight %}

物件的成員變數需要建立物件才能讀寫。

讀取類別靜態變數不用建立物件，只需要類別名與::範圍運算子與變數名就可以讀取靜態類別變數。

```
類別::靜態類別變數
```

```
cout << "count = " << Student::count << endl;
```
完整程式碼
{% highlight c++ linenos %}
class Student {
public:
  static int count;
};
int Student::count = 100;
int main() {
  cout << "count = " << Student::count << endl;
  return 0;
}
{% endhighlight %}

```
count = 100
```

## 靜態類別函式

在函式前面加上static 

語法
```
static void getCount(){};
```

呼叫靜態類別函式不用建立物件，只需要類別名與::範圍運算子與函式名就可以讀取靜態類別函式。

```
類別::靜態類別函式
```

```
Student::getCount();
```

完整程式碼
{% highlight c++ linenos %}
class Student {
public:
  static int count;
  static void getCount() {
    cout << "count = " << count << endl;
  }
};
int Student::count = 100;
int main() {
  Student::getCount();
  return 0;
}
{% endhighlight %}

## 物件可以讀取靜態類別變數與靜態類別函式

物件可以直接讀取靜態類別變數與靜態類別函式

{% highlight c++ linenos %}
class Student {
public:
  static int count;
  static void getCount() {
    cout << "count = " << count << endl;
  }
};
int Student::count = 100;
int main() {
  Student student;
  cout << "student.count = " << student.count << endl;
  student.getCount();
  return 0;
}
{% endhighlight %}
```
student.count = 100
count = 100
```

## 所有物件共享同一個靜態變數

以下的例子即便建了三個物件，但靜態類別變數在記憶體中只有一份，而不是三份，所以三個物件都只在同一個記憶體位址的靜態類別變數進行修改，所以最後一個人修改130，把每個物件的count印出來也全是130。

{% highlight c++ linenos %}
int main() {
  Student student1;
  student1.count = 110;
  
  Student student2;
  student2.count = 120;
  
  Student student3;
  student3.count = 130;
  
  cout << "student1.count = " << student1.count << endl;
  cout << "student2.count = " << student2.count << endl;
  cout << "student3.count = " << student3.count << endl;
  return 0;
}
{% endhighlight %}

```
student1.count = 130
student2.count = 130
student3.count = 130
```

## 靜態類別函式只能讀取靜態類別變數

靜態類別函式只能存取靜態類別變數，不能存取物件成員變數。

以下程式碼，試圖在靜態類別函式(getCount)中存取成員變數(name)，編譯出錯。

{% highlight c++ linenos %}
class Student {
public:
  string name;
  static int count;
  static void getCount() {
    cout << "name =" << name << endl;
    cout << "count = " << count << endl;
  }
};
{% endhighlight %}

## 靜態類別函式無法讀取物件成員函式

以下程式碼，試圖在靜態類別函式(getCount)呼叫成員函式(print)，編譯出錯。

{% highlight c++ linenos %}
class Student {
public:
  string name;
  static int count;
  void print() {
    cout << "name = " << name << endl;
  }
  static void getCount() {
    print();
    cout << "count = " << count << endl;
  }
};
{% endhighlight %}

## 成員函式可以讀取靜態類別函式與靜態類別變數

以下程式碼，在成員函式(print)呼叫靜態類別函式(getCount)與靜態類別變數(count)，不會有問題。

{% highlight c++ linenos %}
class Student {
public:
  string name;
  static int count;
  void print() {
    cout << "name = " << name << endl;
    getCount();
    cout << "2. count = " << count << endl;
  }
  static void getCount() {
    cout << "count = " << count << endl;
  }
};
int Student::count = 100;
int main() {
  Student student1;
  student1.name = "Bill";
  student1.count = 110;
  
  Student student2;
  student2.name = "Mary";
  student2.count = 120;
  
  Student student3;
  student3.name = "Tom";
  student3.count = 130;
  
  student1.print();
  cout << "-----------------" << endl;
  student2.print();
  cout << "-----------------" << endl;
  student3.print();
  return 0;
}
{% endhighlight %}

```
name = Bill
count = 130
2. count = 130
-----------------
name = Mary
count = 130
2. count = 130
-----------------
name = Tom
count = 130
2. count = 130
```

## 靜態類別函式沒有this

因為不是物件，所有不能用this

## private靜態類別變數與private靜態類別函式無法在類別之外被訪問

以下程式碼把count與getCount()移到private

可以使用全域的方式初始化靜態類別變數

```
int Student::count = 100;
```
但以下程式碼試圖在類別之外讀取私有靜態類別變數與私有類別函式，編譯失敗。

完整程式碼
{% highlight c++ linenos %}
class Student {
  static int count;
  static void getCount() {
    cout << "count = " << count << endl;
  }
public:
  string name;
};
int Student::count = 100;
int main() {
  cout << Student::count << endl;
  Student::getCount();
  return 0;
}
{% endhighlight %}



[1]: {% link _pages/c/dynamicMemory/memoryLayout.md %}#bss-segment-記憶體區塊
