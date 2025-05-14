---
title: 泛型
date: 2025-05-06
keywords: Java, Generics
---
所謂的泛型就是除了基本類型之外(int, float, double)，其它的類型都可以，因為基本類型不是物件。

## 語法
{% highlight java linenos %}
class 名稱<T> {

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

## 使用泛型

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