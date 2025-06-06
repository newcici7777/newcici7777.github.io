---
title: 靜態與匿名區塊執行順序
date: 2025-06-06
keywords: java, code block, static code block
---
Prerequisites:

- [static][1]
- [建構子][1]

## 靜態屬性與靜態區塊的執行順序
靜態屬性與靜態區塊的執行順序根據程式碼的前後順序。
{% highlight java linenos %}
class StaticTest {
  static int i = getValue();
  static {
    System.out.println("進到靜態區塊");
  }
  private static int getValue() {
    System.out.println("static 屬性 初始化");
    return 10;
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    System.out.println(StaticTest.i);
  }
}
{% endhighlight %}
```
static 屬性 初始化
進到靜態區塊
10
```

## 靜態區塊使用還未宣告屬性
以下程式碼，靜態區塊先，裡面的i屬性可以不宣告就先使用。
{% highlight java linenos %}
class StaticTest {
  static {
    System.out.println("進到靜態區塊");
    i = 100;
  }
  static int i = getValue();
  private static int getValue() {
    System.out.println("static 屬性 初始化");
    return 10;
  }
}
public class Test2 {
  public static void main(String[] args) {
    System.out.println(StaticTest.i);
  }
}
{% endhighlight %}
```
進到靜態區塊
static 屬性 初始化
10
```

## 靜態屬性與成員屬性的執行順序
以下程式碼靜態屬性與靜態區塊一定先執行，因為是類別要先載入，靜態屬性與靜態區塊是屬於類別。

類別載入完畢，才會執行物件相關。
{% highlight java linenos %}
class StaticTest {
  private int i = getValue();
  static {
    System.out.println("進到靜態區塊");
    si = 100;
  }
  static int si = getSValue();
  private static int getSValue() {
    System.out.println("static 屬性 初始化");
    return 10;
  }
  private static int getValue() {
    System.out.println("成員 屬性 初始化");
    return 100;
  }
}
public class Test2 {
  public static void main(String[] args) {
    new StaticTest();
  }
}
{% endhighlight %}
```
進到靜態區塊
static 屬性 初始化
成員 屬性 初始化
```

## 建構子與成員屬性執行順序
根據程式碼的前後順序，以下是成員屬性先，建構子後。
{% highlight java linenos %}
class StaticTest {
  private int i = getValue();
  StaticTest() {
    System.out.println("建構子 初始化");
  }
  private static int getValue() {
    System.out.println("成員 屬性 初始化");
    return 100;
  }
}
public class Test2 {
  public static void main(String[] args) {
    new StaticTest();
  }
}
{% endhighlight %}
```
成員 屬性 初始化
建構子 初始化
```

## 匿名區塊與建構子執行順序
匿名區塊一定優先建構子。
{% highlight java linenos %}
class StaticTest {
  StaticTest() {
    System.out.println("建構子 初始化");
  }
  {
    System.out.println("匿名區塊 初始化");
  }
}
public class Test2 {
  public static void main(String[] args) {
    new StaticTest();
  }
}
{% endhighlight %}
```
匿名區塊 初始化
建構子 初始化
```

## 匿名區塊與成員屬性執行順序
執行順序根據程式碼的前後順序。

成員屬性在前
{% highlight java linenos %}
class StaticTest {
  private int i = getValue();
  StaticTest() {
    System.out.println("建構子 初始化");
  }
  {
    System.out.println("匿名區塊 初始化");
  }
  private static int getValue() {
    System.out.println("成員 屬性 初始化");
    return 100;
  }
}
public class Test2 {
  public static void main(String[] args) {
    new StaticTest();
  }
}
{% endhighlight %}
```
成員 屬性 初始化
匿名區塊 初始化
建構子 初始化
```

匿名區塊在前
{% highlight java linenos %}
class StaticTest {
  StaticTest() {
    System.out.println("建構子 初始化");
  }
  {
    System.out.println("匿名區塊 初始化");
  }
  private int i = getValue();
  private static int getValue() {
    System.out.println("成員 屬性 初始化");
    return 100;
  }
}
public class Test2 {
  public static void main(String[] args) {
    new StaticTest();
  }
}
{% endhighlight %}
```
匿名區塊 初始化
成員 屬性 初始化
建構子 初始化
```

## 靜態與建構子初始化順序
請問以下程式碼執行順序？
{% highlight java linenos %}
class StaticTest {
  StaticTest() {
    System.out.println("建構子 初始化");
  }
  {
    System.out.println("匿名區塊 初始化");
  }
  private int i = getI();
  private int getI() {
    System.out.println("成員屬性 初始化");
    return 55;
  }
  static int count = getValue();
  static {
    System.out.println("靜態 區塊 初始化");
  }
  private static int getValue() {
    System.out.println("靜態 屬性 初始化");
    return 100;
  }
}
public class Test2 {
  public static void main(String[] args) {
    new StaticTest();
  }
}
{% endhighlight %}
```
靜態 屬性 初始化
靜態 區塊 初始化
匿名區塊 初始化
成員屬性 初始化
建構子 初始化
```

由執行結果可以發現，靜態一定先執行。

## 只使用靜態屬性，建構子會執行嗎？
以下程式碼執行結果是什麼？
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    System.out.println(StaticTest.count);
  }
}
class StaticTest {
  StaticTest() {
    System.out.println("建構子 初始化");
  }
  {
    System.out.println("匿名區塊 初始化");
  }
  private int i = getI();
  private int getI() {
    System.out.println("成員屬性 初始化");
    return 55;
  }
  static int count = getValue();
  static {
    System.out.println("靜態 區塊 初始化");
  }
  private static int getValue() {
    System.out.println("靜態 屬性 初始化");
    return 100;
  }
}
{% endhighlight %}
```
靜態 屬性 初始化
靜態 區塊 初始化
100
```

從結果可以發現只有靜態相關的會執行，而建構子與匿名區塊不會執行。

## 靜態屬性是建構子
以下程式碼，靜態屬性為本身類別的建構子，靜態屬性也是成員屬性，只是儲存靜態屬性的位置是同一個記憶體位址，記憶體位址是共享。

先前提過靜態屬性與靜態區塊的執行順序是根據程式碼前後。

所以以下程式碼會先執行建構子，才執行靜態區塊。
{% highlight java linenos %}
class StaticTest {
  static StaticTest instance = new StaticTest();
  public StaticTest() {
    System.out.println("建構子初始化");
  }
  static {
    System.out.println("靜態區塊初始化");
  }
}
public class Test2 {
  public static void main(String[] args) {
    System.out.println(StaticTest.instance);
  }
}
{% endhighlight %}
```
建構子初始化
靜態區塊初始化
static_.StaticTest@681a9515
```

## 靜態屬性與靜態區塊只會被執行一次
以下程式碼，不管執行幾次，建構子與靜態區塊，只有執行一次。

靜態屬性instance的hashCode都是一模一樣的，hashCode代表一個物件的身份證，是根據記憶體位址換算出來的數字。

{% highlight java linenos %}
class StaticTest {
  static StaticTest instance = new StaticTest();
  public StaticTest() {
    System.out.println("建構子初始化");
  }
  static {
    System.out.println("靜態區塊初始化");
  }
}
public class Test2 {
  public static void main(String[] args) {
    System.out.println(StaticTest.instance.hashCode());
    System.out.println(StaticTest.instance.hashCode());
    System.out.println(StaticTest.instance.hashCode());
    System.out.println(StaticTest.instance.hashCode());
  }
}
{% endhighlight %}
```
建構子初始化
靜態區塊初始化
1746572565
1746572565
1746572565
1746572565
```

## 繼承與靜態屬性
父類別有靜態屬性count，靜態區塊。
{% highlight java linenos %}
class Father {
  static int count = 0;
  static {
    System.out.println("Father static init");
  }
}
{% endhighlight %}

子類別有靜態屬性count，靜態區塊。
{% highlight java linenos %}
class Child extends Father{
  static int count = 5;
  static {
    System.out.println("Child static init");
  }
}
{% endhighlight %}

呼叫子類別的靜態屬性，沒有new，請問靜態區塊如何呼叫嗎？
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    System.out.println(Child.count);
  }
}
{% endhighlight %}
```
Father static init
Child static init
5
```
由執行結果可以發現，父類別先呼叫靜態區塊。

接著呼叫子類別靜態區塊。

## 繼承與子類別方法覆寫。
父類別
{% highlight java linenos %}
class Father {
  int i = getI();
  int getI() {
    System.out.println("Father 屬性初始化");
    return 20;
  }
  Father() {
    System.out.println("Father建構子初始化");
  }
  {
    System.out.println("Father 匿名初始化");
  }
}
{% endhighlight %}

子類別
{% highlight java linenos %}
class Child extends Father{
  int i = getI();
  int getI() {
    System.out.println("Child 屬性初始化");
    return 20;
  }
  Child() {
    System.out.println("Child建構子初始化");
  }
  {
    System.out.println("Child 匿名初始化");
  }
}
{% endhighlight %}

執行測試
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    new Child();
  }
}
{% endhighlight %}
```
Child 屬性初始化  <- 居然先執行？
Father 匿名初始化
Father建構子初始化
Child 屬性初始化
Child 匿名初始化
Child建構子初始化
```

為什麼是`Child 屬性初始化`先執行？因為子類覆寫父類別的getI()方法，所以會執行getI()，實際上是呼叫子類別的getI()。
{% highlight java linenos %}
class Father {
  int i = getI();  // 實際上是呼叫子類別的getI
  int getI() {
    System.out.println("Father 屬性初始化");
    return 20;
  }
}
{% endhighlight %}

若程式碼修正如下，執行結果也就如預期正常。
{% highlight java linenos %}
class Father {
  int i = getFI();
  int getFI() {
    System.out.println("Father 屬性初始化");
    return 20;
  }
  Father() {
    System.out.println("Father建構子初始化");
  }
  {
    System.out.println("Father 匿名初始化");
  }
}

class Child extends Father{
  int i = getI();
  int getI() {
    System.out.println("Child 屬性初始化");
    return 20;
  }
  Child() {
    System.out.println("Child建構子初始化");
  }
  {
    System.out.println("Child 匿名初始化");
  }
}
{% endhighlight %}
```
Father 屬性初始化
Father 匿名初始化
Father建構子初始化
Child 屬性初始化
Child 匿名初始化
Child建構子初始化
```

由執行結果可以發現會先執行父類別的屬性->匿名區塊->建構子，然後才執行子類別。

## 建立物件就呼叫匿名區塊
靜態區塊只會執行一次，匿名區塊跟建構子一樣，每建立一次物件就呼叫一次。
{% highlight java linenos %}
class StaticTest {
  public StaticTest() {
    System.out.println("建構子初始化");
  }

  {
    System.out.println("匿名區塊初始化");
  }

  static {
    System.out.println("靜態區塊初始化");
  }
}
{% endhighlight %}

建立二次物件。
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    new StaticTest();
    new StaticTest();
  }
}
{% endhighlight %}
```
靜態區塊初始化
匿名區塊初始化
建構子初始化
匿名區塊初始化
建構子初始化
```

由執行結果可以發現，靜態區塊只執行一次，匿名區塊與建構子分別執行二次。

執行順序: 靜態區塊 -> 匿名區塊 -> 建構子

[1]: {% link _pages/java/static.md %}#靜態區塊
[2]: {% link _pages/java/constructor.md %}