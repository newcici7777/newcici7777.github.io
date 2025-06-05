---
title: Static
date: 2025-04-19
keywords: Java, static
---
Prerequisites:

- [Memory Model][2]

## 靜態變數
在類別中，除了有成員變數之外，靜態變數也是類別的成員之一。

什麼是靜態變數？靜態變數可以當作共享資源，所有物件都可以共享這個資源。

## 靜態變數的建立
1. Student類別載入至記憶體，會在Metaspace建立metadata，裡面包含有Student類別所有屬性、方法、靜態變數。
2. count靜態變數此時為0。

![img]({{site.imgurl}}/java/static_count.png)

## 靜態變數的Memory Model
建立s1，s1中的count記憶體位址是0x0011，把0x0011記憶體位址中的值\+1

![img]({{site.imgurl}}/java/static_count1.png)

建立s2，s2中的count記憶體位址0x0011，把0x0011記憶體位址中的值\+1

![img]({{site.imgurl}}/java/static_count2.png)

由上圖知道，s1與s2儲存的count的記憶體位址都是0x0011，共用同一個記憶體位址。

建立3個學生，並統計數量。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Student s1 = new Student("Mary");
    Student s2 = new Student("Bill");
    Student s3 = new Student("Andy");
    System.out.println(s1.getName() + ":" + s1.getCount());
    System.out.println(s2.getName() + ":" + s2.getCount());
    System.out.println(s3.getName() + ":" + s3.getCount());
  }
}
class Student {
  private String name;
  private static int count;
  public Student (String name) {
    this.name = name;
    count++;
  }
  public String getName() {
    return name;
  }
  public int getCount() {
    return count;
  }
}
{% endhighlight %}
```
Mary:3
Bill:3
Andy:3
```

## 靜態變數使用方法
語法
```
類別名.靜態變數
物件.靜態變數
```
大部分都是使用「類別名.靜態變數」，在Memory Model中，靜態變數是存放在Class物件中，確實是屬於類別的變數。

使用「物件.靜態變數」，在Memory Model中，物件的靜態變數的記憶體位址，也是指向Class物件中靜態變數的記憶體位址。

{% highlight java linenos %}
public class Test {
  public static String static_name = "靜態變數";
  public static void main(String[] args) {
    System.out.println(Test.static_name);
  }
}
{% endhighlight %}
```
靜態變數
```

## 靜態方法
語法
```
類別名.靜態方法()
```
靜態方法屬於類別的方法，只能存取靜態變數與靜態方法，通常作為工具類別，進行跟物件無關的操作。
{% highlight java linenos %}
public class Test {
  public static void staticMethod1() {
    System.out.println("靜態方法");
  }
  public static void main(String[] args) {
    Test.staticMethod1();
  }
}
{% endhighlight %}
```
靜態方法
```
## 靜態區塊
類別載入的時候，會在Metaspace中建立metadata，裡面有一個clinit()，靜態區塊的程式碼會以inline的方式合併在clinit()函式中。

```
.class 檔 → ClassLoader 載入類別
       ↓
   建立 metadata（Metaspace） 和 java.lang.Class（Heap）
       ↓
   Linking（連接）
     ├── Verification（驗證）
     ├── Preparation（配置 static 區域預設值）
     └── Resolution 
       ↓
Initialization（執行 clinit）
  → static 區塊與 static 欄位被初始化
```

|Linking階段|描述|
|:-----|:-------------|
|Verification|確保 class 檔案格式正確、邏輯正確（例如 stack 不會溢出，方法合法)|
|Preparation|分配 static 欄位的空間，設初始值（int 為 0、Object 為 null）不執行 static 區塊|
|Resolution| 把 symbolic reference（如 Lcom/example/Foo;）轉換為實際reference（即記憶體指標）|

Initialization : static 區塊與 static 欄位被初始化，例如: static int var = 23

語法
{% highlight java linenos %}
  static {
    System.out.println("靜態區塊");
  }
{% endhighlight %}

[建構子][1]的文章中，提到建立物件會先呼叫「匿名區塊」，才呼叫建構子。

那如果加上靜態區塊？誰先執行？答案是靜態區塊先執行，然後才是匿名區塊，最後是建構子

{% highlight java linenos %}
public class Test {
  // 靜態區塊
  static {
    System.out.println("靜態區塊");
  }

  {
    System.out.println("匿名區塊");
  }

  Test() {
    System.out.println("建構子區塊");
  }

  public static void main(String[] args) {
    Test test1 = new Test();
  }
}
{% endhighlight %}
```
靜態區塊
匿名區塊
建構子區塊
```

## 靜態只產生一次
把前一個程式碼的main主程式改成下方，會發現靜態區塊只產生一次。
{% highlight java linenos %}
  public static void main(String[] args) {
    Test test1 = new Test();
    Test test2 = new Test();
  }
{% endhighlight %}
```
靜態區塊
匿名區塊
建構子區塊
匿名區塊
建構子區塊
```

由以上結果可知，執行2次new建立物件，但「靜態區塊」只會產生一次，不管建立多少次物件，靜態變數、靜態方法、靜態區塊，只會產生一次。

## 靜態方法只能存取靜態變數、靜態方法
{% highlight java linenos %}
public class StaticExample {
  public static String static_name = "static_name";
  public String name = "name";
  public static void staticMethod1() {
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void staticMethod2() {
    System.out.println("呼叫 staticMethod2()");
  }

  public static void main(String[] args) {
    StaticExample.staticMethod1();
  }
}
{% endhighlight %}
```
呼叫靜態方法:
呼叫 staticMethod2()
印出靜態變數:static_name
```

## 成員方法可以存取靜態變數、靜態方法
{% highlight java linenos %}
public class StaticExample {
  public static String static_name = "static_name";
  public String name = "name";

  public static void staticMethod1() {
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void staticMethod2() {
    System.out.println("呼叫 staticMethod2()");
  }

  public void method1() {
    System.out.println("印出成員變數:" + name);
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void main(String[] args) {
    StaticExample example = new StaticExample();
    example.method1();
  }
}
{% endhighlight %}
```
印出成員變數:name
呼叫靜態方法:
呼叫 staticMethod2()
印出靜態變數:static_name
```

## main主程式也是靜態方法
main主程式也是靜態方法，可以直接存取類別的靜態方法。

{% highlight java linenos %}
public class StaticExample {
  public static String static_name = "static_name";
  public String name = "name";

  public static void staticMethod1() {
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void staticMethod2() {
    System.out.println("呼叫 staticMethod2()");
  }

  public void method1() {
    System.out.println("印出成員變數:" + name);
    System.out.println("呼叫靜態方法:");
    staticMethod2();
    System.out.println("印出靜態變數:" + static_name);
  }

  public static void main(String[] args) {
    staticMethod1();
  }
}
{% endhighlight %}
```
呼叫靜態方法:
呼叫 staticMethod2()
印出靜態變數:static_name
```

[1]: {% link _pages/java/constructor.md %}
[2]: {% link _pages/java/memory_model.md %}
