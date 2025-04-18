---
title: 多型
date: 2025-04-17
keywords: Java, polymorphism, Upcasting object, Downcasting object
---
## 向上轉型物件Upcasting object
使用子類別的建立物件的建構子，「產生出父類別的物件」，稱之為向上轉型物件Upcasting object。

以下的方式，都是產生出向上轉型物件，注意！這裡不是產生子類物件，而是產生「向上轉型物件」。

### 建立向上轉型物件，方式1
{% highlight java linenos %}
Parent parent;
Child child = new Child();
parent = child;
{% endhighlight %}

### 建立向上轉型物件，方式2
{% highlight java linenos %}
Parent parent;
parent = new Child();
{% endhighlight %}

### 建立向上轉型物件，方式3
{% highlight java linenos %}
Parent parent = new Child();
{% endhighlight %}

## 向上轉型物件存取範圍
向上轉型物件，無法呼叫子類別自己擁有成員變數與成員方法，只能呼叫子類別繼承的方法與變數，以及子類別「override覆寫」父類別的方法與變數。

## 向上轉型物件不等於父類別
由子類別建立的向上轉型物件，不等於父類別。

父類別可以存取自己的成員變數與成員方法，子類別也可以存取自己的成員變數與成員方法，但向上轉型物件無法讀取自己的成員變數與成員方法，因此向上轉型物件不等於父類別也不等於子類別。

## 向上轉型物件範例1
以下Parent類別有name與age成員變數，與2個建構子。
{% highlight java linenos %}
public class Parent {
  String name;
  int age;
  Parent(){}
  Parent(String name) {
    this.name = name;
  }
}
{% endhighlight %}

Child類別除了繼承父類，有name與age成員變數，與2個建構子。  
Child類別自己有address成員變數與getAddress()與setAddress()二個成員方法，並非來自繼承得到。
{% highlight java linenos %}
public class Child extends Parent{
  String address;
  public String getAddress() {
    return address;
  }
  public void setAddress(String address) {
    this.address = address;
  }
  Child(String name) {
    this.name = "child_" + name;
  }
}
{% endhighlight %}

建立向上轉型物件
{% highlight java linenos %}
Parent upcasting_obj = new Child("Alice");
{% endhighlight %}

upcasting_obj是向上轉型物件，即使是用「子類別的建構子」產生的「父類別物件」，但是upcasting_obj向上轉型物件缺少了address成員變數與getAddress()與setAddress()二個成員方法，但有覆寫的Child(String name)建構子，與覆寫的name成員變數。

驗證向上轉型物件擁有繼承來的name變數
{% highlight java linenos %}
public class Teest {
  public static void main(String[] args) {
    Parent upcasting_obj = new Child("Alice");
    // 擁有子類別覆寫的建構子Child(String name)
    // 擁有繼承來的name變數
    System.out.println(upcasting_obj.name);
    // 以下的都無法呼叫子類別自己擁有的成員變數與成員方法
    //upcasting_obj.setAddress("xxx");
    //upcasting_obj.getAddress();
    //System.out.println(upcasting_obj.address);
  }
}
{% endhighlight %}
```
child_Alice
```

## 向上轉型物件範例2
人
{% highlight java linenos %}
public class People {
    public void speak() {}
}
{% endhighlight %}

美國人
{% highlight java linenos %}
public class American extends People{
    @Override
    public void speak() {
        System.out.println("speak English.");
    }
    public void goChurch() {
        System.out.println("go to Church.");
    }
}
{% endhighlight %}

中國人
{% highlight java linenos %}
public class Chinese extends People{
    @Override
    public void speak() {
        System.out.println("說中文");
    }
    public void tombSweeping() {
        System.out.println("掃墓");
    }
}
{% endhighlight %}

日本人
{% highlight java linenos %}
public class Japanese extends People{
    @Override
    public void speak() {
        System.out.println("說日文");
    }
    public void dollFestival() {
        System.out.println("女兒節.");
    }
}
{% endhighlight %}

建立向上轉型物件，僅能呼叫子類別覆寫父類別的speak()方法，向上轉型物件不能呼叫子類別特有的goChurch(),tombSweeping(),dollFestival()
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    People upcasting_obj1 = new Chinese();
    upcasting_obj1.speak();
    People upcasting_obj2 = new American();
    upcasting_obj2.speak();
    People upcasting_obj3 = new Japanese();
    upcasting_obj3.speak();
  }
}
{% endhighlight %}
```
說中文
speak English.
說日文
```

## 向下轉型物件
將父類別物件向下轉型成子類別。

向上轉型的定義是，使用子類別的建立物件的建構子，「產生出父類別的物件」。

這次我們將把「產生出父類別的物件」，向下轉型成子類別。

### 自動轉型
由於向上轉型只會有一個父類別，因為每個類別只能繼承一個父類別，因此使用自動轉型。

{% highlight java linenos %}
Parent parent;
Child child = new Child();
// 以下是自動轉型
parent = child;
{% endhighlight %}

### 強制轉型
父類別下面會有多個子類別，不確定要轉成那個子類別，因此採用強制轉型，也就是使用括號，括號中是要轉型的(子類別)。
{% highlight java linenos %}
People upcasting_obj1 = new Chinese();
// 強制轉型成子類別
Chinese chinese = (Chinese) upcasting_obj1;
{% endhighlight %}

### 強轉成子類物件
強轉回子類物件又會擁有子類別特有的方法。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    People upcasting_obj1 = new Chinese();
    upcasting_obj1.speak();
    People upcasting_obj2 = new American();
    upcasting_obj2.speak();
    People upcasting_obj3 = new Japanese();
    upcasting_obj3.speak();

    System.out.println("------強轉成子類別----------");
    Chinese chinese = (Chinese) upcasting_obj1;
    chinese.tombSweeping();
    American american = (American) upcasting_obj2;
    american.goChurch();
    Japanese japanese = (Japanese) upcasting_obj3;
    japanese.dollFestival();
  }
}
{% endhighlight %}
```
說中文
speak English.
說日文
------強轉成子類別----------
掃墓
go to Church.
女兒節.
```

## 多型
所謂的多型就是，父類別可以指向多種不同子類別，父類別可以使用「子類別覆寫父類別的方法」。  
範例如下
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    People people = new People();
    // 父類別指向美國人
    people = new American();
    // 使用子類別覆寫父類別的方法
    people.speak();

    // 父類別指向中國人
    people = new Chinese();
    // 使用子類別覆寫父類別的方法
    people.speak();

    // 父類別指向日本人
    people = new Japanese();
    // 使用子類別覆寫父類別的方法
    people.speak();
  }
}
{% endhighlight %}
```
speak English.
說中文
說日文
```