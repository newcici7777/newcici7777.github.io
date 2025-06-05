---
title: final
date: 2025-06-04
keywords: Java, final
---
## final修飾的範圍
區域中的變數、全域變數、類別、屬性、方法。

不能放在建構子前面。

## final 常數
final放在變數前，代表變數的值不能被修改，而且一定要給預設值。

因為值不能被修改，所以不是變數，是常數。

{% highlight java linenos %}
final String msg = "Test";
// 下面會編譯錯誤
msg = "ABC";
{% endhighlight %}

參數x是final，但下面程式碼不會有錯誤，因為並沒有修改到x。
{% highlight java linenos %}
public int add(final int x) {
  return  x + 1;
}
{% endhighlight %}

若使用x\+\+就會有錯誤，因為x\+\+就是`x = x + 1`，是修改x常數，但是final不能被修改。
{% highlight java linenos %}
public int add(final int x) {
  //
  x++;
  return  x + 1;
}
{% endhighlight %}

## final與陣列
Prerequisites:

- [陣列記憶體模型][1]

final對於陣列而言，變數是不能再指向其它陣列。
{% highlight java linenos %}
final char[] arr1 = {'H', 'e', 'l', 'l', 'o'};
char[] arr2 = {'W', 'o', 'r', 'l', 'd'};
// 下面會編譯錯誤，arr1不能再指向其它陣列。
arr1 = arr2;
{% endhighlight %}

但可以修改陣列裡的內容。
{% highlight java linenos %}
final char[] arr1 = {'H', 'e', 'l', 'l', 'o'};
arr1[0] = 'W';
arr1[1] = 'o';
arr1[2] = 'r';
arr1[3] = 'l';
arr1[4] = 'd';
System.out.println(arr1);
{% endhighlight %}
```
World
```
## 類別中的final
### 常數
final放在屬性前，就變成常數，因為不能再改變，而且一定要給預設值。

final常數，不能再被修改，繼承的子類別也不能修改final常數，本身類別也不能修改final常數，只被設定一次。

常數名為大寫，用底線作分隔，例如: IMG_PATH

{% highlight java linenos %}
public class Test {
  public final String IMG_URL = "http://xxxxxxx";
}
{% endhighlight %}

### 在下面幾種情況，final可以不設預設值
#### 不是static的final
final原本一定要設預設值。
{% highlight java linenos %}
public final String IMG_URL = "http://xxxxxxx";
{% endhighlight %}

下面的狀況下，可以不用設預設值。

在建構子中會設定final的值，final就可以不設預設值。
{% highlight java linenos %}
public class Test {
  public final String IMG_URL;

  public Test(String IMG_URL) {
    this.IMG_URL = IMG_URL;
  }
}
{% endhighlight %}

在匿名區塊會設定final的值，final就可以不設預設值。
{% highlight java linenos %}
public class Test {
  public final String IMG_URL;
  
  {
    IMG_URL = "http://xxxxxx";
  }
}
{% endhighlight %}

#### final static
final static 原本一定要設預設值。
{% highlight java linenos %}
public final static String IMG_URL = "http://xxxxxx";
{% endhighlight %}

在以下的狀況，可以不用設預設值。

在靜態區塊會設定final static的值，final static就可以不設預設值。
{% highlight java linenos %}
public class Test {
  public final static String IMG_URL;
  static {
    IMG_URL = "http://xxxxxx";
  }
{% endhighlight %}

### final \+ static 不會呼叫靜態區塊
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(WebSite.IMG_URL);
  }
}
class WebSite {
  public final static String IMG_URL = "http://xxxxxx";
  static {
    System.out.println("靜態區塊初始化");
  }
}
{% endhighlight %}
```
http://xxxxxx
```

由執行結果可以發現，不會印出"靜態區塊初始化"，類別也不會被classLoader載入到JVM記憶體中，就不會佔用記憶體空間。

去掉final，只剩下static。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(WebSite.IMG_URL);
  }
}
class WebSite {
  // 去掉final，只剩下static
  public static String IMG_URL = "http://xxxxxx";
  static {
    System.out.println("靜態區塊初始化");
  }
}
{% endhighlight %}
```
靜態區塊初始化
http://xxxxxx
```

由執行結果可以發現，會印出"靜態區塊初始化"，類別會被classLoader載入到JVM記憶體中，會佔用記憶體空間。

在Kotlin中，有一種延遲初始化的功能，也就是要用的時候，才會被初始化，才會佔用記憶體，而static \+ final更強大，呼叫的時候，也不會佔用記憶體空間。

## final 類別
為什麼會有final類別呢？有一些基礎的類別，不想再被人繼承，也不想被人覆寫方法。String、Double、Integer...都是final 類別。

### 不能被繼承
{% highlight java linenos %}
// 父類別不能被繼承
final class Parent {
}
{% endhighlight %}

子類別繼承final父類別，會編譯錯誤。
{% highlight java linenos %}
// 以下程式碼會編譯錯誤
class Child extends Parent {
}
{% endhighlight %}

### 可以建立物件
final Parent
{% highlight java linenos %}
final class Parent {
}
{% endhighlight %}

建立Parent物件。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Parent parent = new Parent();
  }
}
{% endhighlight %}

### final類別中的方法不用寫final
因為final類別不能被繼承，不會有子類別，所以不可能有子類別覆寫父類別方法。

以下的寫法畫蛇添足。
{% highlight java linenos %}
final class Parent {
  public final void method() {
  }
}
{% endhighlight %}

## final 方法
不能被子類別覆寫方法，但子類別可以使用父類別的方法。

父類別
{% highlight java linenos %}
class Parent {
  public final void method() {
    System.out.println("父類別方法1");
  }
}
{% endhighlight %}

子類別嘗試覆寫method()，會編譯錯誤。
{% highlight java linenos %}
class Child extends Parent {
  // 以下程式碼編譯錯誤
  public final void method() {
    System.out.println("子類別方法1");
  }
}
{% endhighlight %}

子類別可以使用父類別方法。
{% highlight java linenos %}
class Child extends Parent {
  public final void method1() {
    // 使用父類別方法
    super.method();
  }
}
{% endhighlight %}





[1]: {% link _pages/java/memory_model.md %}#陣列記憶體位址複製
