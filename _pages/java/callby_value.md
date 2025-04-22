---
title: 方法傳值Call by value
date: 2025-04-18
keywords: Java, call by value
---
在Java中，只有Call by value，然後要忘記c++的知識，java沒有像c++有call by address, call by reference。

Java的reference，與C++的reference是完全不同的理念。

方法就是函式，也就是物件中所定義的函式，物件定義的函式就叫作成員方法，也叫作成員函式，二者是一樣的意思。

什麼是call by value?

參數一進入方法，方法即建立新的變數保管這些參數值，同時新變數的資料型態完全吻合參數的資料型態，這種傳遞參數的方式會先把參數複製一份，稱為傳值呼叫(call by value)。

## 修改成員變數

以下的程式碼要傳遞testClz變數給函式changeClz()，實際上是複製testClz記憶體位址給arg1的「區域」變數(參考下圖)。

![img]({{site.imgurl}}/java/reference1.png)

找到age的記憶體位址，並修改裡面的值為100。
{% highlight java linenos %}
arg1.age = 100;
{% endhighlight %}

對於物件記憶體位址有興趣，可以參考c++物件記憶體布局:

- [物件記憶體布局 Memory layout][1]

{% highlight java linenos %}
  arg1.age = 100;    
{% endhighlight %}    

{% highlight java linenos %}
public class Teest {
  public static void main(String[] args) {
    Teest test = new Teest();
    TestClz testClz = new TestClz();
    testClz.age = 50;
    test.changeClz(testClz);
    System.out.println(testClz.age);
  }
  public void changeClz(TestClz arg1) {
    arg1.age = 100;
  }
}
class TestClz {
  public int age;
}
{% endhighlight %}
```
100
```

## 修改參數的記憶體位址
以下的程式碼要傳遞testClz變數給函式changeClzRef()，實際上是複製testClz記憶體位址給arg1的「區域」變數。

接著arg1區域變數原本指向0x12345678，改成指向0x12345689
{% highlight java linenos %}
  arg1 = new TestClz();
{% endhighlight %}

![img]({{site.imgurl}}/java/reference1.png)

此時此刻，arg1區域變數與testClz變數指的都不是相同物件，所以執行結果是50，而不是印出60，區域變數arg1離開changeClzRef()方法，就立即被記憶體釋放。

{% highlight java linenos %}
public class Teest {
  public static void main(String[] args) {
    Teest test = new Teest();
    TestClz testClz = new TestClz();
    testClz.age = 50;
    test.changeClzRef(testClz);
    System.out.println(testClz.age);
  }
  public void changeClzRef(TestClz arg1) {
    arg1 = new TestClz();
    arg1.age = 60;
  }
}
class TestClz {
  public int age;
}
{% endhighlight %}
```
50
```

此處的概念跟C++的[call by address][2]與[call by reference][3]完全不同，是不一樣的概念，請勿混為一談。

[1]: {% link _pages/c/class/inheritance.md %}
[2]: {% link _pages/c/function/func_param_pointer.md %}
[3]: {% link _pages/c/function/callByRef.md %}