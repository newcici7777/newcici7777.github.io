---
title: Call by value
date: 2025-04-18
keywords: Java, call by value
---
一進入方法，若傳進來的參數是物件，也就是記憶體位址，方法建立新的變數拷貝傳進來的參數，這種方式會先把參數複製一份，稱為Call by value。

## 修改成員變數
以下的程式碼要傳遞testClz變數給copyAddress()方法。
TestClz
{% highlight java linenos %}
class TestClz {
  public int age;
}
{% endhighlight %}

Test
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Test test = new Test();
    TestClz testClz = new TestClz();
    testClz.age = 50;
    // 把testClz作為參數傳進copyAddress()
    test.copyAddress(testClz);
    System.out.println(testClz.age);
  }
  public void copyAddress(TestClz arg1) {
    arg1.age = 100;
  }
}
{% endhighlight %}

「複製」testClz變數「記憶體位址」給arg1變數(參考下圖)。  
- arg1存的0x0033
- testClz變數存的0x0033

![img]({{site.imgurl}}/java/reference1.png)

arg1存的0x0033與testClz變數存的0x0033一樣，所以修改arg1.age，也就是修改0x0033記憶體位址中的物件。
{% highlight java linenos %}
arg1.age = 100;
{% endhighlight %}

執行結果為100
```
100
```

## 參數設成新的記憶體位址
TestClz
{% highlight java linenos %}
class TestClz {
  public int age;
}
{% endhighlight %}

Test
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Test test = new Test();
    TestClz testClz = new TestClz();
    testClz.age = 50;
    test.copyAddress(testClz);
    System.out.println(testClz.age);
  }
  public void copyAddress(TestClz arg1) {
    // 參數設成新的記憶體位址
    arg1 = new TestClz();
    arg1.age = 60;
  }
}
{% endhighlight %}

參數arg1存其它記憶體位址。
{% highlight java linenos %}
arg1 = new TestClz();
{% endhighlight %}

arg1存的0x0066與testClz變數存的0x0033不一樣，所以修改arg1.age，也就是修改0x0066記憶體位址中的物件，而不是修改0x0033記憶體中的物件。

![img]({{site.imgurl}}/java/reference2.png)

執行結果是50
```
50
```

執行完copyAddress()方法後，參數arg1就會被記憶體回收，包含0x0066也會被記憶體回收。  

下圖中，離開copyAddress()方法後，Stack堆疊已經沒有arg1參數，Heap堆也沒有0x0066。

![img]({{site.imgurl}}/java/reference3.png)

此處是「拷貝記憶體位址」到「參數」中，並非「指向」記憶體位址，跟C++的[call by address][1]與[call by reference][2]完全不同的概念。

畫圖檔案名稱為func_copy_address.drawio

[1]: {% link _pages/c/function/func_param_pointer.md %}
[2]: {% link _pages/c/function/callByRef.md %}