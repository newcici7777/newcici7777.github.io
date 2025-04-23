---
title: UML類別圖
date: 2025-04-21
keywords: Java, UML
---
## 類別圖製作工具

<https://app.diagrams.net/>

- [安裝Amateras][1]

使用Eclipse的plugin: Amateras

## 成員屬性與方法增加
此處的成員屬性就是成員變數，二者意思相同  
此處的成員方法就是成員函數，二者意思相同  

Add Attribute增加成員變數  
Add Operation增加方法  

![img]({{site.imgurl}}/java/amateras13.png)

## 成員屬性與方法
注意！冒號`:`後面會空一格

屬性
```
名稱: 類型
name: String 
```

方法
```
方法名(參數名: 參數類型): 傳回值類型
setName(name: String): void
getName(): String
```

![img]({{site.imgurl}}/java/uml1.png)

## 存取權限
```
+ public
- private
# protected
~ default(package)
```

![img]({{site.imgurl}}/java/access_level.png)

本篇類別圖如同「Head First Design Patterns」深入淺出設計模式一書中，都不用存取權限修飾子符號。

## 關係
依賴、組合、聚合，都是在描述類別與類別之間的關係。

## 依賴
在類別中「用到」其它類別就是依賴。

A類別「用到」B類別，箭頭圖:A類別 -> B類別

圖中灰色的箭頭線  
![img]({{site.imgurl}}/java/dependency.png)

依賴有以下方式:
- 方法參數
- 方法傳回值
- 區域變數

A類別
{% highlight java linenos %}
public class A {
  // 方法參數用到B類別
  void doBmethod(B b) {
    b.method1();
  }
}
{% endhighlight %}

B類別
{% highlight java linenos %}
public class B {
  void method1() {
    System.out.println("B method1 do something.");
  }
  void method2() {
    System.out.println("B method2 do something.");
  }
  void method3() {
    System.out.println("B method3 do something.");
  }
}
{% endhighlight %}

## 聚合
聚合的方式，使用成員屬性與setter方法

下圖中，A類有一個B的屬性與setB()方法，空心菱形圖案代表聚合，把B聚合給A

![img]({{site.imgurl}}/java/aggregation.png)

A類別
{% highlight java linenos %}
public class A {
  B b;
  void setB(B b) {
    this.b = b;
  }
}
{% endhighlight %}

B類別與上述的一樣，不再另寫程式碼。

## 組合
組合的意思代表一個類別建立，也會把另一個類別建立，二個類別不可分開。

建立A類別的時候，也把B類別new出來，並把B類別設給成員屬性。  
A類別建構子中建立B類別，也是屬於組合的關係。

下圖中，A類別有一個B的屬性，黑色實心菱形圖案代表組合，把B組合給A
![img]({{site.imgurl}}/java/composite.png)

A類別
{% highlight java linenos %}
public class A {
  B b = new B();
}
{% endhighlight %}

B類別與上述的一樣，不再另寫程式碼。

## 關聯關係
有箭頭代表示單方面擁有這個類別，沒箭頭代表二邊都擁有彼此的類別。

### 1對1單向(有箭頭)

圖中會在線的二端各寫上1，屬性名稱-b

![img]({{site.imgurl}}/java/relation1.png)

A類別擁有B類別成員屬性
{% highlight java linenos %}
public class A {
  B b; // A類別擁有B類別成員屬性
}
{% endhighlight %}

B類別沒有A類別成員屬性
{% highlight java linenos %}
public class B {

}
{% endhighlight %}

### 1對1雙向(沒箭頭)

圖中會在線的二端各寫上1，屬性名稱-a與-b

![img]({{site.imgurl}}/java/relation2.png)

A類別擁有B類別成員屬性
{% highlight java linenos %}
public class A {
  B b; // A類別擁有B類別成員屬性
}
{% endhighlight %}

B類別擁有A類別成員屬性
{% highlight java linenos %}
public class B {
  A a; // B類別擁有A類別成員屬性
}
{% endhighlight %}

## 繼承
線條是實線  
![img]({{site.imgurl}}/java/extends.png)

A類別
{% highlight java linenos %}
public class A {
}
{% endhighlight %}

B類別
{% highlight java linenos %}
public class B extends A{
}
{% endhighlight %}

## 實作
線條為虛線  
![img]({{site.imgurl}}/java/interface_implements.png)

A類別
{% highlight java linenos %}
interface A {
}
{% endhighlight %}

B類別
{% highlight java linenos %}
public class B implements A{
}
{% endhighlight %}

## 抽象類別覆寫
線條為虛線  
![img]({{site.imgurl}}/java/abstract.png)

{% highlight java linenos %}
public abstract class Shape {
  abstract void draw();
}

public class Circle extends Shape {
  @Override
  void draw() {
    System.out.println("畫一個圓");
  }
}
{% endhighlight %}

[1]: {% link _pages/java/amateras.md %}