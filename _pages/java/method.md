---
title: 方法
date: 2025-07-01
keywords: Java, method
---
## 語法
傳回值為void，return可以省略不寫。
```
public void 方法名(參數) {
	陳述式;
	return;
}
```

傳回值不為void，return一定要寫。
```
public 傳回值類型 方法名(參數) {
	陳述式;
	return 傳回值;
}
```

## return
return代表離開所在方法。

若傳回值是void，預設是省略不寫return，但在運作時會有一個隱藏的return，程式碼沒有return，但程式設計師心中要知道有一個隱藏的return。

{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    TestObj testObj = new TestObj();
    // 呼叫方法
    testObj.method1();
  }
}
class TestObj {
  // void方法
  public void method1() {
    System.out.println("test");
    // 預設是省略不寫
    return;
  }
}
{% endhighlight %}

## 方法返回的位置
方法返回的位置十分十分重要！以前我一直以為void方法返回的位置是在呼叫方法的下一行，後來才知道，方法返回位置是在原本呼叫方法的地方。
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    TestObj testObj = new TestObj();
    // 呼叫方法
    testObj.method1();
  }
}
class TestObj {
  // void方法
  public void method1() {
    System.out.println("test");
    // 預設是省略不寫
    return;
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/java/method1.png)

## Memory Model
### main方法呼叫方法
程式碼使用上一個例子。
1. main()方法本身就是程式的入口，呼叫main()方法會在Stack堆疊空間建立一個記憶體空間。
2. main()方法呼叫testObj.method1()方法。
3. 在Stack建立一個method1()方法空間。
4. 印出test後，執行return返回到main()方法呼叫method1()的位罝。
5. 離開method1()方法後，記憶體空間會被記憶體回收。

![img]({{site.imgurl}}/java/method2.png)

### 方法呼叫方法程式碼
以下程式碼，main() -> 呼叫 -> method1() -> 呼叫 -> method2() → 呼叫 -> method3() -> 呼叫 -> method4() <br>
記憶體空間怎麼配置？<br>
最後印出結果為何？<br>
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    TestObj testObj = new TestObj();
    testObj.method1();
    System.out.println("main");
  }
}
class TestObj {
  public void method1() {
    method2();
    System.out.println("test1");
    return;
  }
  public void method2() {
    method3();
    System.out.println("test2");
    return;
  }
  public void method3() {
    method4();
    System.out.println("test3");
    return;
  }
  public void method4() {
    System.out.println("test4");
    return;
  }
}
{% endhighlight %}

### 方法呼叫方法執行過程
以下圖片已加上執行順序編號，藍色的編號為印出螢幕的順序。<br>

![img]({{site.imgurl}}/java/method3.png)

return最終回到main()方法，method1()、method2()、method3()、method4()記憶體空間會被記憶體回收，但為了展示執行過程，圖中的記憶體空間先保留，實際狀況是return離開當下的方法，當下的方法就會被記憶體回收。

main()方法執行完畢，return離開main()方法，整個程式也執行完畢。

執行結果。
```
test4
test3
test2
test1
main
```
## 方法傳遞值
Java的方法傳遞值，只有call by value，完全不可能會有call by address。
### 傳遞基本型態參數
main()方法建立2個變數x與y，並把x與y傳入method1(x, y)。
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    int x = 10, y = 20;
    TestObj2 testObj2 = new TestObj2();
    testObj2.method1(x, y);
  }
}
class TestObj2 {
  public void method1(int x, int y) {
    System.out.println(x + y);
    return;
  }
}
{% endhighlight %}
```
30
```
#### 複製數字
方法傳遞基本型態，就是「複製數字」到method1方法。<br>
方法內部會自動建立區域變數x與y，並把複製的數字，指派到x與y中。<br>
注意！區域變數x，y的生命週期只有method1方法內，離開method1方法就會被記憶體回收。<br>
method1區域變數x，y與main()方法中的x，y是完全不同。<br>

![img]({{site.imgurl}}/java/method4.png)

#### 無法修改基本型態參數
方法傳遞基本型態，就是「複製數字」到method1方法。<br>
所以根本無法垮越方法修改到main()方法的x與y變數，method1方法中的x與y變數生命周期與存取範圍只在method1方法中，return離開method1方法，method1方法中的x與y變數也會被記憶體釋放，根本不可能活著到返回main()方法。
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    int x = 10, y = 20;
    TestObj2 testObj2 = new TestObj2();
    testObj2.method1(x, y);
    System.out.println("x = " + x + ", y = " + y);
  }
}
class TestObj2 {
  public void method1(int x, int y) {
    x = 100;
    y = 200;
    return;
  }
}
{% endhighlight %}
```
x = 10, y = 20
```

### 傳遞類別
Prerequisites:

- [callby_value][1]

再次強調一次，Java的方法傳遞類別，是把「記憶體位址」「複製」到method1方法中str1變數，「複製記憶體位址」是call by value。

method1方法的str1變數存的記憶體位址跟main方法的str1記憶體位址是一樣。
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    String str1 = "abcd";
    TestObj2 testObj2 = new TestObj2();
    testObj2.method1(str1);
    System.out.println("str1 = " + str1);
  }
}
class TestObj2 {
  public void method1(String str1) {
    str1 = "abcdef";
    return;
  }
}
{% endhighlight %}
```
str1 = abcdef
```

![img]({{site.imgurl}}/java/method5.png)

#### 方法中修改參數位址
以下程式碼，方法把參數位址修改成null。
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    String str1 = "abcd";
    TestObj2 testObj2 = new TestObj2();
    testObj2.method1(str1);
    System.out.println("str1 = " + str1);
  }
}
class TestObj2 {
  public void method1(String str1) {
    str1 = null;
    return;
  }
}
{% endhighlight %}
```
str1 = abcd
```

Java的方法傳遞類別，是把「記憶體位址」「複製」到method1方法中str1變數的值。<br>
method1方法中的str1變數是區域變數，設為null，也只是對method1區域變數中的str1的值改變，對main()方法中的str1根本沒影嚮。

method1方法中的str1變數生命周期與存取範圍只在method1方法中，return離開method1方法，method1方法中的str1變數也會被記憶體釋放，根本不可能活著到返回main()方法。

method1區域變數str1與main()方法中的str1是完全不同，是個別獨立。<br>

![img]({{site.imgurl}}/java/method6.png)

## return 傳回值
如果有return 傳回值，那就會把方法中的值，傳回main方法，並且「要有變數去接收」傳回值。

{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    String str1 = "abcd";
    TestObj2 testObj2 = new TestObj2();
    str1 = testObj2.method1(str1);
    System.out.println("str1 = " + str1);
  }
}
class TestObj2 {
  public String method1(String str1) {
    str1 = null;
    return str1;
  }
}
{% endhighlight %}
```
str1 = null
```

如果沒有變數去接收傳回值，仍是不會變更main()方法的str1變數。
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    String str1 = "abcd";
    TestObj2 testObj2 = new TestObj2();
    testObj2.method1(str1);
    System.out.println("str1 = " + str1);
  }
}
class TestObj2 {
  public String method1(String str1) {
    str1 = null;
    return str1;
  }
}
{% endhighlight %}
```
str1 = abcd
```

## return 傳回值與多個方法呼叫
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    TestObj testObj = new TestObj();
    int res = testObj.method1(10);
    System.out.println("res = " + res);
  }
}
class TestObj {
  public int method1(int n) {
    int res = method2(n) + 1;
    return res;
  }
  public int method2(int n) {
    int res = method3(n) + 1;
    return res;
  }
  public int method3(int n) {
    int res = method4(n) + 1;
    return res;
  }
  public int method4(int n) {
    return n + 1;
  }
}
{% endhighlight %}
```
14
```

方法呼叫與return，傳回值如下圖所示。

![img]({{site.imgurl}}/java/method7.png)


[1]: {% link _pages/java/callby_value.md %}