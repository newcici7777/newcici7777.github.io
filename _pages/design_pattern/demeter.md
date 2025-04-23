---
title: 最少知道原則
date: 2025-04-23
keywords: Java, Design patterns, Law of Demeter
---
Prerequisites:

- [類別圖][1]
- [多型][2]

## 迪米特原則 

英文是Law of Demeter

迪米特原則也就是最少知道原則。

什麼是最少知道原則？假設A類別用到B類別的方法，A類別不會想知道B類別的內部結構以及程式是如何寫的，B類別只要提供一個public的方法給A類呼叫就好。

## 朋友關係
A類別用到B類別，所謂的「用到」，可能是依賴、聚合、組合、關連的關係，以下都是朋友關係。

- 方法參數(依賴)
- setter(聚合)
- 成員變數(組合或關聯)
- 傳回值

區域變數就不是朋友關係。

根據最少知道原則，類別與類別之間的關係，一定要是朋友關係。

## 朋友關係範例
Student類別
{% highlight java linenos %}
class Student {
  String name;
  Student() {}
  Student(String name) {
    this.name = name;
  }
}
{% endhighlight %}

對於getAllStudent()方法中，傳回值類型有Student，所以Student是朋友。
{% highlight java linenos %}
class StudentManager {
  List<Student> getAllStudent() {
    List<Student> list = new ArrayList<>();
    for (int i = 0; i < 10 ; i++) {
      Student student = new Student("student" + i);
      list.add(student);
    }
    return list;
  }
}
{% endhighlight %}

## 不是朋友範例
參數StudentManager是printAll()方法的朋友
{% highlight java linenos %}
class PrintManager {
  void printAll(StudentManager studentManager) {
    List<Student> students = studentManager.getAllStudent();
    for (int i = 0; i < students.size(); i++) {
      System.out.println(students.get(i).name);
    }
  }
}
{% endhighlight %}

但是區域變數List<Student>，Student既不是方法參數也不是傳回值類型，所以Student不是朋友。
{% highlight java linenos %}
List<Student> students = studentManager.getAllStudent();
{% endhighlight %}

## 如何修正？
先前有提到「A類別用到B類別的方法，A類別不會想知道B類別的內部結構以及程式是如何寫的，B類別只要提供一個public的方法給A類呼叫就好。」

但是PrintManager.printAll()，居然取出studentManager中的學生清單，違反「不會知道其它類別的內部結構」。

把印出學生姓名的程式碼，放回StudentManager，提供一個public的printAllStudent()方法給別人呼叫。
{% highlight java linenos %}
class StudentManager {
  List<Student> getAllStudent() {
    List<Student> list = new ArrayList<>();
    for (int i = 0; i < 10 ; i++) {
      Student student = new Student("student" + i);
      list.add(student);
    }
    return list;
  }

  // 印出學生
  void printAllStudent() {
    List<Student> students = this.getAllStudent();
    for (int i = 0; i < students.size(); i++) {
      System.out.println(students.get(i).name);
    }
  }
}
{% endhighlight %}

PrintManager修改成，透過朋友，也就是參數studentManager，去印出學生
{% highlight java linenos %}
class PrintManager {
  void printAll(StudentManager studentManager) {
    studentManager.printAllStudent();
  }
}
{% endhighlight %}


[1]: {% link _pages/java/uml.md %}
[2]: {% link _pages/java/polymorphism.md %}