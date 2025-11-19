---
title: 泛型類別
date: 2025-05-06
keywords: Java, Generics
---
## 自訂類型
{% highlight java linenos %}
int i;

i = 100;
i = 55;
i = 22;
{% endhighlight %}
i是變數。

以上等號右邊的值，可以為100、55、22。

{% highlight java linenos %}
T = Integer;
T = Double;
T = ArrayList;
T = Object;
{% endhighlight %}
T為Type類型的英文字母。

T是變數。

以上等號右邊的值，可以為Integer, Double, ArrayList, Object。

所以下面文章內容所有的T, R, K, V ...，任何英文字母，請把它看作變數，而變數中的值，就是類型。

## 泛型類別
在類別名後面，用尖括號包住\<自訂類型\>。

自訂類型T, R, M 可用英文字母任意一個大寫字母。

{% highlight java linenos %}
class Cat<T, R, M> {
}
{% endhighlight %}

## 屬性
泛型類別的自訂類型，可以用在屬性類型。
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  private R r;
  private M m;
}
{% endhighlight %}

## 建構子
泛型類別的自訂類型，可以用在建構子參數類型。
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  private R r;
  private M m;

  public Cat(T t, R r, M m) {
    this.t = t;
    this.r = r;
    this.m = m;
  }
}
{% endhighlight %}

## get,set方法
泛型類別的自訂類型，可以用在方法參數類型與傳回值類型。
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  private R r;
  private M m;

  public T getT() {
    return t;
  }

  public void setT(T t) {
    this.t = t;
  }

  public R getR() {
    return r;
  }

  public void setR(R r) {
    this.r = r;
  }

  public M getM() {
    return m;
  }

  public void setM(M m) {
    this.m = m;
  }

{% endhighlight %}

## 宣告與建立物件編譯器才會知道類型
{% highlight java linenos %}
Cat<String, Integer, Dog> cat = new Cat<String, Integer, Dog>();
{% endhighlight %}

jdk8以後可以省略後面的尖括號中的類型，因為前面已經宣告泛型類別(T,R,M)的類型是什麼，後面的可由前面的尖括號內容進行自動推導。
{% highlight java linenos %}
Cat<String, Integer, Dog> cat = new Cat<>();
{% endhighlight %}

## 沒泛型類別的類型，預設Object
沒設定泛型類別的類型。
{% highlight java linenos %}
Cat cat = new Cat();
{% endhighlight %}

編譯器自動認為泛型類別的類型是Object。
{% highlight java linenos %}
Cat<Object, Object, Object> cat = new Cat<>();
{% endhighlight %}

## 泛型類別可以使用子類別
類別B繼承類別A
{% highlight java linenos %}
class A {}
class B extends A {}
{% endhighlight %}

Student是泛型類別，有類型T屬性，建構子參數為類型T。
{% highlight java linenos %}
class Student<T> {
  private T t;

  public Student(T t) {
    this.t = t;
  }
}
{% endhighlight %}

由以下的範例可以知道，泛型類別的類型是可以用子類別。
{% highlight java linenos %}
  Student<A> s3 = new Student<>(new A());
  // T類型是父類別A，但建構子放的是子類別B
  Student<A> s4 = new Student<>(new B());
{% endhighlight %}

以下的程式碼也都是正確的，ArrayList是List的子類別，也是Object的子類別。
{% highlight java linenos %}
  Student<List> s1 = new Student<>(new ArrayList<>());
  Student<Object> s2 = new Student<>(new ArrayList<>());
{% endhighlight %}

## 基本型別自動轉成包裝類別

- [包裝類別][2]

貓咪類別
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  private R r;
  private M m;

  public Cat(T t, R r, M m) {
    this.t = t;
    this.r = r;
    this.m = m;
  }
}
{% endhighlight %}

指定泛型類別的類型，並建立物件，注意！建構子的參數都是基本型態，但內部會自動把12.5轉成Double類別，1轉成Integer類別。
{% highlight java linenos %}
Cat<Double, Boolean, Integer> cat = new Cat<>(12.5, true, 1);
{% endhighlight %}

## 使用getClass()知道泛型類別的類型
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t;
  private R r;
  private M m;

  public Cat(T t, R r, M m) {
    this.t = t;
    this.r = r;
    this.m = m;
  }
  
  public void showType() {
    System.out.println(t.getClass());
    System.out.println(r.getClass());
    System.out.println(m.getClass());
  }
}
public class Test {
  public static void main(String[] args) {
    Cat<Double, Boolean, Integer> cat = new Cat<>(12.5, true, 1);
    cat.showType();
  }
}
{% endhighlight %}
```
class java.lang.Double
class java.lang.Boolean
class java.lang.Integer
```

## 不能使用泛型類別的情況
### 泛型類別不可以new
以下二種new是不被允許，因為編譯器根本不知道T是什麼類型，又怎麼會知道要在記憶體建立多大空間來裝這個物件。
{% highlight java linenos %}
class Cat<T, R, M> {
  private T t = new T();
  private T[] arr = new T[10];
}
{% endhighlight %}

### 泛型類別不可以用在靜態變數與靜態方法

- [memory_model][1]
- [static][3]

靜態變數是在Class物件中，Class物件比物件更早建立，泛型類別要在物件建立的時候才指定類型，但靜態物件早就建立好了，根本不可能指定靜態變數的類型。

以下三種都不能使用。
{% highlight java linenos %}
class Cat<T, R, M> {
  // static屬性
  public static T t;
  
  // static方法
  public static T getStaticT() {
  }
  
  // static方法
  public static void setStaticT(T t) {
  }
}
{% endhighlight %}

### 不能用基本型態
基本型態有short, int, long, double, float, boolean, char

以下內容編譯不過。
{% highlight java linenos %}
Cat<int, double, char> cat = new Cat<>();
{% endhighlight %}

------------------------------------------------------------

以下是另一種筆記。

## 語法
在類別名後面，自訂類型。

自訂類型的名稱可用T, R, K, V 任意一個大寫字母。
{% highlight java linenos %}
class 類別名<T, R, U> {

}
{% endhighlight %}

T代表Type，也就是類型。

T可以是成員變數的類型，也可以是成員方法參數類型。

## 成員變數類型
{% highlight java linenos %}
class 名稱<T> {
  T 變數名;
}
{% endhighlight %}

{% highlight java linenos %}
class Obj1<T> {
  T id;
}
{% endhighlight %}

## 參數類型
{% highlight java linenos %}
class Obj2<T> {
  public void showMsg(T msg) {
    msg.toString();
  }
}
{% endhighlight %}

類型為T，只能用toString()的方法，toString()是由Object類別(所有類別的父類別)所提供的。

## 傳回值類型
{% highlight java linenos %}
class Obj3<T> {
  private T[] elements;
  public T getElements(int index) {
    return elements[index];
  }
}
{% endhighlight %}

## 使用泛型類別
### 宣告
在類別名後面有尖括號，尖括號中寫上實際的類型。
{% highlight java linenos %}
類別名<類型> 變數;
Obj1<String> test;
{% endhighlight %}

### 建立物件
new 類別名後面有尖括號，尖括號中寫上實際的類型。

語法
{% highlight java linenos %}
new 類別名<類型>();
{% endhighlight %}

JDK7以前一定要寫類型
{% highlight java linenos %}
Obj1<String> test = new Obj1<String>();
{% endhighlight %}

JDK7就可以不用在尖括號中寫類型，因為編譯器可以從宣告的時候就推斷類型。
{% highlight java linenos %}
Obj1<String> test = new Obj1<>();
{% endhighlight %}

## 範例
Cat類別，重寫toString()的方法
{% highlight java linenos %}
class Cat {
  @Override
  public String toString() {
    return "貓貓貓";
  }
}
{% endhighlight %}

Dog類別，重寫toString()的方法
{% highlight java linenos %}
class Dog {
  @Override
  public String toString() {
    return "狗狗狗";
  }
}
{% endhighlight %}

ShowObj，使用泛型，參數是泛型，什麼類型都可以。
{% highlight java linenos %}
class ShowObj<T> {
  public void showObj(T obj) {
    System.out.println(obj.toString());
  }
}
{% endhighlight %}

宣告ShowObj是Cat類型，showObj()方法參數只能是Cat類型，不能放Dog類型，不然編譯時會出錯。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    ShowObj<Cat> show1 = new ShowObj<>();
    show1.showObj(new Cat());
  }
}
{% endhighlight %}
```
貓貓貓
```

宣告ShowObj是Dog類型，showObj()方法參數只能是Dog類型，不能放Cat類型，不然編譯時會出錯。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    ShowObj<Dog> show2 = new ShowObj<>();
    show2.showObj(new Dog());
  }
}
{% endhighlight %}
```
狗狗狗
```

[1]: {% link _pages/java/memory_model.md %}#物件建立過程
[2]: {% link _pages/java/wrap.md %}
[3]: {% link _pages/java/static.md %}