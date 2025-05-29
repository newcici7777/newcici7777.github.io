---
title: 繼承
date: 2025-05-29
keywords: Java, Extends
---
## 繼承概念
為什麼要用到繼承？當多個類別有相同的屬性與方法，可以把這些相同的屬性與方法抽出來做為父類別，子類別繼承父類別就擁有那些相同的屬性與方法，解決程式碼重覆。

以下A類別與B類別都有3個相同屬性與方法。
{% highlight java linenos %}
class A {
  private String s1;
  private String s2;
  private String s3;
  private String sa;
  public void method1() {}
  public void method2() {}
  public void method3() {}
  public void methodA() {}       
}
{% endhighlight %}

{% highlight java linenos %}
class B {
  private String s1;
  private String s2;
  private String s3;
  private String sb;
  public void method1() {}
  public void method2() {}
  public void method3() {}
  public void methodB() {}      
}
{% endhighlight %}

把相同屬性與方法抽出來變成父類別。
{% highlight java linenos %}
class Parent {
  private String s1;
  private String s2;
  private String s3;
  public void method1() {}
  public void method2() {}
  public void method3() {}    
}
{% endhighlight %}

A類別與B類別繼承Parent，就可以跟之前一樣，各自擁有s1,s2,s3屬性與method1,method2,method3方法，A類別與B類別又各自保有跟其它類別不相同的屬性與方法，相同的抽取出來作為父類別，不同的作為子類別。
{% highlight java linenos %}
class A extends Parent{
  private String sa;
  public void methodA() {}       
}
{% endhighlight %}

{% highlight java linenos %}
class B extends Parent{
  private String sb;
  public void methodB() {}      
}
{% endhighlight %}

## 繼承關係階層圖
先把滑鼠移到子類別名。

![img]({{site.imgurl}}/java/hierarchy1.png)

按下ctrl + h

![img]({{site.imgurl}}/java/hierarchy2.png)

## Memory Layout

- [Object Layout core][1]

使用Object Layout core，可以看出物件記憶體內部屬性。

{% highlight java linenos %}
import org.openjdk.jol.info.ClassLayout;
public class Test {
  public static void main(String[] args) {
    System.out.println(ClassLayout.parseInstance(new A()).toPrintable());
    System.out.println(ClassLayout.parseInstance(new B()).toPrintable());
  }
}
class Parent {
  private String s1;
  private String s2;
  private String s3;
  public void method1() {}
  public void method2() {}
  public void method3() {}
}

class A extends Parent{
  private String sa;
  public void methodA() {}
}

class B extends Parent{
  private String sb;
  public void methodB() {}
}
{% endhighlight %}

執行結果可以看出A物件與B物件裡面有3個屬性(s1,s2,s3)來自Parent類別，只有一個屬性是自己的。
<pre>
inherit.<span class="markline">A object</span> internals:
OFF  SZ               TYPE DESCRIPTION               VALUE
  0   8                    (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4                    (object header: class)    0x01003410
<span class="markline">  
 12   4   java.lang.String Parent.s1                 null
 16   4   java.lang.String Parent.s2                 null
 20   4   java.lang.String Parent.s3                 null
 24   4   java.lang.String A.sa                      null
</span> 
 28   4                    (object alignment gap)    
Instance size: 32 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total

inherit.<span class="markline">B object</span> internals:
OFF  SZ               TYPE DESCRIPTION               VALUE
  0   8                    (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4                    (object header: class)    0x0108c460
<span class="markline">   
 12   4   java.lang.String Parent.s1                 null
 16   4   java.lang.String Parent.s2                 null
 20   4   java.lang.String Parent.s3                 null
 24   4   java.lang.String B.sb                      null
</span>  
 28   4                    (object alignment gap)    
Instance size: 32 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total
</pre>

## 取得子類別所有方法
- [取得父類別所有方法][2]

使用以下程式碼取得A類別所有方法。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws ClassNotFoundException {
    // 參數是「package.類別名」
    getClzMethod(Class.forName("inherit.A"));
  }

  public static void getClzMethod(Class<?> clz) {
    Class<?> clazz = clz;
    System.out.println("Methods in class: " + clazz.getName());
    for (Method method : clazz.getDeclaredMethods()) {
      System.out.println(method);
    }
    System.out.println("------------------");
    // parent
    Class<?> superclass = clazz.getSuperclass();
    System.out.println("Methods in Parent: " + clazz.getName());
    for (Method method : superclass.getDeclaredMethods()) {
      System.out.println(method);
    }
  }
}
{% endhighlight %}
```
Methods in class: inherit.A
public void inherit.A.methodA()
------------------
Methods in Parent: inherit.A
public void inherit.Parent.method1()
public void inherit.Parent.method2()
public void inherit.Parent.method3()
````
由以上執行結果，可以看出A類有methodA()。

其它method1()、method2()、method3()，都來自父類別。

## 呼叫誰的方法，就用誰的屬性
父類別有一個屬性i = 20
{% highlight java linenos %}
class Parent {
  private String s1;
  private String s2;
  private String s3;
  int i = 20;  // 父類別屬性i

  // 父類別方法
  public void method1() {
    // 印出父類別屬性i
    System.out.println("i = " + i);
  }

  public void method2() {
  }

  public void method3() {
  }
}
{% endhighlight %}

子類別i = 10，並且覆寫method1()。
{% highlight java linenos %}
class A extends Parent {
  int i = 10;  // 子類別屬性i
  private String sa;

  @Override
  public void method1() {
    // 印出子類別屬性i，如果子類別沒有屬性i，就用父類別的屬性i
    System.out.println("i = " + i);
  }

  public void methodA() {
    // 使用super.呼叫父類別的method()
    super.method1();
  }
}
{% endhighlight %}

請問執行子類別的methodA()，會印出什麼？
{% highlight java linenos %}
public class Test {
  public static void main(String[] args){
    A test = new A();
    test.methodA();
  }
}  
{% endhighlight %}
```
i = 20
```
執行結果是印出父類別屬性i = 20，而不是子類別i = 10。


請問執行子類別覆寫的method1()，會印出什麼？
{% highlight java linenos %}
public class Test {
  public static void main(String[] args){
    A test = new A();
    test.method1();
  }
}  
{% endhighlight %}
```
i = 10
```
執行結果為10。

由此可證，呼叫父類別的方法，使用的是父類別屬性，呼叫子類別的方法，使用的是子類別屬性，除非子類別沒這個屬性，就會從父類別找有沒有這個屬性。

## 覆寫
以上A類別有覆寫method1()方法，什麼是覆寫?就是父類別原本有的方法，子類別寫一模一樣的方法覆蓋過去。
{% highlight java linenos %}
  @Override
  public void method1() {
    System.out.println("i = " + i);
  }
{% endhighlight %}

使用之前[取得子類別所有方法](#取得子類別所有方法)的程式碼。

執行結果如下:
```
Methods in class: inherit.A
public void inherit.A.method1()
public void inherit.A.methodA()
------------------
Methods in Parent: inherit.A
public void inherit.Parent.method1()
public void inherit.Parent.method2()
public void inherit.Parent.method3()
```
可以看出子類別有method1()與methodA()。

父類別也有method1()，那二邊都有method1()，要執行誰的？

根據等號=右邊是誰，就執行誰的方法，除非子類別沒有那個方法，才執行父類別的方法。
{% highlight java linenos %}
  A test = new A();
  test.method1();  // 執行A類別下的method11()
{% endhighlight %}

## 使用Debug工具，列出物件所有屬性
把斷點設在new 子類別()的下一行，滑鼠移到變數，本例是變數是test，會顯示物件所有屬性，包含父類別屬性，相同屬性，會用父類別名.屬性來區分。下圖中是用Parent.i，告知這是父類別的i屬性。
![img]({{site.imgurl}}/java/extend1.png)

## 不用透過super取得父類別的屬性與方法
父類別的屬性與方法，子類別可以直接使用，不用透過super。

以下程式碼，子類別直接用n1,n2,method1()，沒有透過super使用父類別屬性方法。
{% highlight java linenos %}
// 父類別
class Father {
  int n1;
  int n2;
  private int n3;
  
  public void method1() {}
  private void method2() {}
}

// 子類別
class Child extends Father{
  public void show() {
    System.out.println(n1);
    System.out.println(n2);
    method1();
  }
}
{% endhighlight %}

父類別與子類別都有一樣的方法或屬性，就會用super來指定是要用父類別的，還是子類別的，若沒有寫super，預設用子類別。

子類別無法存取父類別private屬性與方法。

## 父類別私有屬性或方法，透過父類別public方法取得
即便子類別繼承父類別，但是private屬性與方法沒辦法存取。

父類別提供public的方法，給子類呼叫。
{% highlight java linenos %}
class Father {
  int n1;
  int n2;
  private int n3;

  public void method1() {}
  private void method2() {}

  // 取得私有屬性
  public int getN3() {
    return n3;
  }

  // 呼叫私有方法
  public void callMethod2() {
    method2();
  }
}
{% endhighlight %}

子類別透過public方法，取得private屬性與呼叫private方法。
{% highlight java linenos %}
class Child extends Father{
  public void show() {
    System.out.println(getN3());
    callMethod2();
  }
}
{% endhighlight %}




[1]: {% link _pages/java/obj_layout_core.md %}
[2]: {% link _pages/java/reflect.md %}#取得父類別所有方法
