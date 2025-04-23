---
title: 開放關閉原則
date: 2025-04-23
keywords: Java, Design patterns, Open Close Principle
---
## 開放關閉原則

英文是Open Close Principle

對修改關閉，對擴充開放。

意思是增加一個新類別，不用修改程式碼。

會變動的程式碼、細節的部分，寫在實作的具體類別，不會變動程式碼寫在抽象類別或介面。

## 未優化前程式碼

下圖，有一個形狀Shape的類別，Circle、Rectangle、Square都繼承Shape

CreateShape類別中，drawCircle()，drawRectangle()與drawSquare()方法中的參數用到Shape類別。

![img]({{site.imgurl}}/pattern/open_close1.png)

程式碼如下:

形狀相關的程式碼
{% highlight java linenos %}
public class Shape {
  int type = 0;
}

public class Circle  extends Shape{
  Circle() {
    super.type = 1;
  }
}

public class Square  extends Shape{
  Square() {
    super.type = 2;
  }
}

public class Rectangle extends Shape{
  Rectangle() {
    super.type = 3;
  }
}
{% endhighlight %}

CreateShape
{% highlight java linenos %}
public class CreateShape {
  void drawShap(Shape shape) {
    if (shape.type == 1)
      drawCircle();
    else if (shape.type == 2)
      drawSquare();
    else if (shape.type == 3)
      drawRectangle();
  }

  void drawCircle() {
    System.out.println("圓形");
  }
  
  void drawRectangle() {
    System.out.println("長方形");
  }
  
  void drawSquare() {
    System.out.println("正方形");
  }
}
{% endhighlight %}

main主程式
{% highlight java linenos %}
  public static void main(String[] args) {
    CreateShape createShape = new CreateShape();
    createShape.drawShap(new Circle());
  }
{% endhighlight %}
```
圓形
```

## 問題
請問，若要加上一個三角形，會動到那些程式碼？

開放關閉原則是增加一個新類別，不用修改程式碼，但以上的程式碼要大幅度修改CreateShape這個類別。

## 優化程式碼
把以上會變動、細節部分的程式碼寫在實作的類別裡，不會變動的部分就寫在抽象裡。

Shape變成抽象類別，有一個抽象方法draw()，至於怎麼畫的細節就由子類別去覆寫draw()方法。

CreateShape只要呼叫Shape.draw()就好了，畫draw()這個「行為」是不會變的，至於怎麼畫怎麼變動的細節就是子類別處理。

![img]({{site.imgurl}}/pattern/open_close2.png)

形狀類別程式碼
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

public class Rectangle extends Shape{
  @Override
  void draw() {
    System.out.println("畫一個長方形");
  }
}

public class Square extends Shape{
  @Override
  void draw() {
    System.out.println("畫一個正方形");
  }
}
{% endhighlight %}

CreateShape
{% highlight java linenos %}
public class CreateShape {
  void drawShap(Shape shape) {
    shape.draw();
  }
}
{% endhighlight %}

Main主程式
{% highlight java linenos %}
public static void main(String[] args) {
  CreateShape createShape = new CreateShape();
  createShape.drawShap(new Circle());
}
{% endhighlight %}
```
畫一個圓
```

## 新增三角形
最後，再問同個問題，新增一個三角形Triangle，要改動那些程式碼？答案是，不用改！只要建立一個Triangle類別並繼承Shape，覆寫draw()方法，把怎麼畫三角形都細節全寫進去。

{% highlight java linenos %}
public class Triangle extends Shape{
  @Override
  void draw() {
    System.out.println("畫一個三角形");
  }
}
{% endhighlight %}

Main主程式
{% highlight java linenos %}
public static void main(String[] args) {
  CreateShape createShape = new CreateShape();
  createShape.drawShap(new Triangle());
}
{% endhighlight %}
```
畫一個三角形
```