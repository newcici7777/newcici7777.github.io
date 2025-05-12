---
title: 匿名類別
date: 2025-04-18
keywords: Java, Anonymous Classes, Inner class
---
Prerequisites:
- [多型][1]
- [抽象類別][2]
- [介面][3]

## 抽象類別的匿名子類別
### 建立抽象類別Human，與抽象方法speak()

Human.java
{% highlight java linenos %}
abstract class Human {
  abstract void speak();
}
{% endhighlight %}

### 建立一個簡單的類別TestHuman，類別中有一個test()方法，參數是Human類別，用來測試多型，只要是Human的子類別，都可以作為參數傳入。

TestHuman.java
{% highlight java linenos %}
public class TestHuman {
  void test(Human human) {
    human.speak();
  }
}
{% endhighlight %}

### 建立Main主程式
用new的方式，建立TestHuman物件，並使用test()方法
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    TestHuman testHuman = new TestHuman();
    testHuman.test(要在這裡建立匿名類別);
  }
}
{% endhighlight %}

### 匿名的方式建立子類別1
建立匿名子類別步驟如下:  
1.new 抽象類別() {}  
要用 `new 抽象類別()`，最重重要的是，後面要加上花括號{}

2.在test()方法中，加上步驟1的參數`new 抽象類別() {}`  
{% highlight java linenos %}
  testHuman.test(new 抽象類別 () {});
{% endhighlight %}

3.在花括號中，覆寫(Override)抽象類別Human的抽象方法speak()  
{% highlight java linenos %}
  testHuman.test(new Human() {
    @Override
    void speak() {

    }
  });
{% endhighlight %}

4.加上要覆寫的內容  
{% highlight java linenos %}
  testHuman.test(new Human() {
    @Override
    void speak() {
      // 覆寫的內容
      System.out.println("說中文");
    }
  });
{% endhighlight %}
```
說中文
```

### 匿名花括號{}
在上一段程式碼利用花括號{}建立沒有名字的繼承抽象類別的子類別，可以視為花括號{}中的內容是子類別的body。
```
{
  子類別的body
}
```

### 匿名的方式建立子類別2
1. 宣告japan變數為父類別Human
2. new 父類別Human()
3. 加上花括號{}
4. 覆寫speak方法
此時japan就是Human的子類別
{% highlight java linenos %}
    Human japan = new Human() {
      @Override
      void speak() {
        System.out.println("說日文");
      }
    };
    japan.speak();
{% endhighlight %}
```
說日文
```

### 匿名的方式建立子類別3
1. new 父類別Human()
2. 加上花括號{}
3. 覆寫speak方法
{% highlight java linenos %}
  new Human() {
    @Override
    void speak() {
      System.out.println("說日文");
    }
  }.speak();
{% endhighlight %}
```
說日文
```

## 類別的匿名子類別
除了抽象類別建立的匿名類別，一般類別也可以透過匿名的方式建立子類別

建立一個Car類別，裡面有一個print()方法。
{% highlight java linenos %}
public class Car {
  public void print() {
    System.out.println("car");
  }
}
{% endhighlight %}

建立「匿名」Car的子類別，注意！加上花括號{}，表達的不是Car這個類別，而是繼承Car的子類別。
{% highlight java linenos %}
public static void main(String[] args) {
  Car toyota = new Car() {
    @Override
    public void print() {
      System.out.println("我是Toyota");
    }
  };
  toyota.print();
}
{% endhighlight %}
```
我是Toyota
```

{% highlight java linenos %}
public static void main(String[] args) {
  new Car() {
    @Override
    public void print() {
      System.out.println("我是Toyota");
    }
  }.print();
}
{% endhighlight %}
```
我是Toyota
```

## 方法參數為匿名類別
建立一個測試參數為Car的類別，TestCar.java
{% highlight java linenos %}
public class TestCar {
  public void test(Car car) {
    car.print();
  }
}
{% endhighlight %}

方法參數為「匿名」Car的子類別，注意！加上花括號{}，表達的不是Car這個類別，而是繼承Car的子類別，即便{}花括號的內容是空的，這裡的意思是子類別，不是Car這個類別，Car變成父類別，這個沒有名字的子類別擁有父類別繼承來的print()方法。
{% highlight java linenos %}
public static void main(String[] args) {
  TestCar testCar = new TestCar();
  // 建立Car的子類別
  testCar.test(new Car() {});
}
{% endhighlight %}
```
car
```

覆寫(重寫)繼承Car的子類別的print()方法
{% highlight java linenos %}
public static void main(String[] args) {
  TestCar testCar = new TestCar();
  testCar.test(new Car() {
    @Override
    public void print() {
      System.out.println("我是Toyota");
    }
  });
}
{% endhighlight %}
```
我是Toyota
```

## 介面(Interface)的匿名類別
建立一個Fly介面
{% highlight java linenos %}
public interface Fly {
  void fly();
}
{% endhighlight %}

介面匿名步驟:
1. new 介面()
2. 加上花括號{}
3. 覆寫fly方法

透過new 介面() {} 的方式，建立介面的匿名類別

方式1:
{% highlight java linenos %}
new Fly() {
    @Override
    public void fly() {
      System.out.println("蚊子飛");
    }  
}.fly();
{% endhighlight %}
```
蚊子飛
```

方式2:
{% highlight java linenos %}
public static void main(String[] args) {
  Fly mosquito = new Fly() {
    @Override
    public void fly() {
      System.out.println("蚊子飛");
    }
  };
  mosquito.fly();
}
{% endhighlight %}
```
蚊子飛
```

[1]: {% link _pages/java/polymorphism.md %}
[2]: {% link _pages/java/abstract.md %}
[3]: {% link _pages/java/interface.md %}
