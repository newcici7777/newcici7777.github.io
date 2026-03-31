---
title: 類別
date: 2026-03-31
keywords: flutter, dart, class, constructor
---
## 類別語法
```
類別 變數名 = 類別(參數名: 值, 參數名: 值);
```

## 建構子
語法
```
類別名({可選命名參數}) {

}
```

{% highlight dart linenos %}
void main() {
  Cat cat1 = Cat(name: "Bill", age: 1);
  print("cat1 name: ${cat1.name}, age: ${cat1.age}");
}

class Cat {
  String? name;
  int? age;
  Cat({String? name, int? age}) {
    this.name = name;
    this.age = age;
  }
}
{% endhighlight %}
```
cat1 name: Bill, age: 1
```

## 類別名.函式名({可選命名參數})
語法:<br>
```
類別名.函式名({可選命名參數}) {

}
```
{% highlight dart linenos %}
void main() {
  Cat cat1 = Cat(name: "Bill", age: 1);
  print("cat1 name: ${cat1.name}, age: ${cat1.age}");
  Cat cat2 = Cat.create(name: "Mary", age: 10);
  print("cat2 name: ${cat2.name}, age: ${cat2.age}");
}

class Cat {
  String? name;
  int? age;
  Cat({String? name, int? age}) {
    this.name = name;
    this.age = age;
  }
  Cat.create({String? name, int? age}) {
    this.name = name;
    this.age = age;
  }
}
{% endhighlight %}
```
cat1 name: Bill, age: 1
cat2 name: Mary, age: 10
```

## 精簡建構子
語法:<br>
```
建構子({this.屬性1, this.屬性2});
類別名.函式名({this.屬性1, this.屬性2});
```

{% highlight dart linenos %}
void main() {
  Cat cat1 = Cat(name: "Bill", age: 1);
  print("cat1 name: ${cat1.name}, age: ${cat1.age}");
  Cat cat2 = Cat.create(name: "Mary", age: 10);
  print("cat2 name: ${cat2.name}, age: ${cat2.age}");
}

class Cat {
  String? name;
  int? age;
  Cat({this.name, this.age});
  Cat.create({this.name, this.age});
}
{% endhighlight %}
```
cat1 name: Bill, age: 1
cat2 name: Mary, age: 10
```

## 私有屬性 私有方法
前面加上底線就變成私有，只能在自己類別內使用，外部無法讀取，除非提供公有方法給外部讀取。<br>

語法<br>
```
類型? _屬性名;

傳回值類型 _方法名() {

}
```

## 繼承
dart只有單繼承，沒有多繼承。<br>

語法
```
class 子類別 extends 父類別 {

}
```

### 繼承中的建構子
dart 跟 Java、Python 不一樣，子類別不會自動繼承父類別的建構子。<br>

以下程式碼會編譯(語法)錯誤，因為 Child 沒有建構子。<br>
{% highlight dart linenos %}
void main() {
  Child child1 = Child(name: "Bill", age: 1);
  child1.printInfo();
}

class Parent {
  String? name;
  int? age;
  Parent({this.name, this.age});
  void printInfo() {
    print("name: $name, age: $age");
  }
}

class Child extends Parent {}
{% endhighlight %}

子類別繼承父類別的建構子:
```
子類別({可選命名參數}) : super(父類別屬性1:命名參數, 父類別屬性2:命名參數);
```

修改後的子類別建構子。<br>
{% highlight dart linenos %}
void main() {
  Child child1 = Child(name: "Bill", age: 1);
  child1.printInfo();
}

class Parent {
  String? name;
  int? age;
  Parent({this.name, this.age});
  void printInfo() {
    print("name: $name, age: $age");
  }
}

class Child extends Parent {
  Child({String? name, int? age}) : super(name: name, age: age);
}
{% endhighlight %}

## override 覆寫
子類別覆寫父類別的方法
{% highlight dart linenos %}
void main() {
  Child child1 = Child(name: "Bill", age: 1);
  child1.printInfo();
}

class Parent {
  String? name;
  int? age;
  Parent({this.name, this.age});
  void printInfo() {
    print("name: $name, age: $age");
  }
}

class Child extends Parent {
  Child({String? name, int? age}) : super(name: name, age: age);
  @override
  void printInfo() {
    print("Chid name: $name, age: $age");
  }
}
{% endhighlight %}
```
Chid name: Bill, age: 1
```

## 多型
多型的方式有二種:<br>
- override 方法覆寫
- 抽象類別

### override 方法覆寫
1. 子類別覆寫父類別的方法。<br>
2. 父類別 變數 = 子類別()
3. 變數.呼叫子類別覆寫父類別的方法()

類型雖然是父類別，但實際指向的物件是子類別。<br>
{% highlight dart linenos %}
void main() {
  Animal dog = Dog();
  dog.eat();
  Animal cat = Cat();
  cat.eat();
}

class Animal {
  eat() {
    print("Animal eating");
  }
}

class Dog extends Animal {
  @override
  eat() {
    print("Dog eating");
  }
}

class Cat extends Animal {
  @override
  eat() {
    print("Cat eating");
  }
}
{% endhighlight %}
```
Dog eating
Cat eating
```

### 抽象類別
抽象類別中的方法不實作，但繼承的子類別一定要實作。<br>
抽象類別不能被建立。<br>

多型的步驗:
1. abstract class 父類別
2. 宣告抽象方法()，()後面是分號，沒有內容。
3. 子類別 implements 父類別
4. override 覆寫抽象方法

{% highlight dart linenos %}
void main() {
  Animal dog = Dog();
  dog.eat();
  Animal cat = Cat();
  cat.eat();
}

abstract class Animal {
  void eat();
}

class Dog implements Animal {
  @override
  eat() {
    print("Dog eating");
  }
}

class Cat implements Animal {
  @override
  eat() {
    print("Cat eating");
  }
}

{% endhighlight %}
```
Dog eating
Cat eating
```



