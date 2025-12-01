---
title: Classloader類別載入
date: 2025-06-05
keywords: java, class loader
---
Prerequisites:

- [metadata][1]

## 什麼時候會載入類別？
使用到靜態屬性、呼叫靜態方法、使用到靜態區塊、使用new建立物件。

以下程式碼使用到靜態屬性StaticTest.i，就會呼叫Class Loader載入類別到記憶體。
{% highlight java linenos %}
class StaticTest {
  static int i = 100;
}
public class Test2 {
  public static void main(String[] args) {
    System.out.println(StaticTest.i);
  }
}
{% endhighlight %}

## 類別載入至記憶體
```
Loading 載入.class
       ↓
建立 metadata（Metaspace） 和 java.lang.Class（Heap）
```

1. 會在metaspace建立metadata，裡面包含有類別所有屬性、方法、靜態變數…等等
2. 接著在Heap建立一個Class物件，裡面也有屬性、方法、建構子…等等，但實際都是指向metadata的資料。

![img]({{site.imgurl}}/java/obj_model1.png)<br>

## 載入順序
大步驟是以下三個，但Linking連接，裡面又有驗證、準備、解析三步驟。<br>
- Loading 載入
- Linking 連接
- Initialization 初始化

![img]({{site.imgur  l}}/java/load_step.png)<br>

```
1.Loading 載入.class檔
    ↓
   建立 metadata（Metaspace）和 java.lang.Class（Heap）
    ↓
2.Linking（連接）
  ├── 2.1 Verification 驗證
  ├── 2.2 Preparation 準備（設置static屬性預設值）
  └── 2.3 Resolution 解析 (設置static屬性的初始值)
    ↓
3.Initialization 初始化（執行 clinit）
   → static 區塊與 static 欄位被初始化
```
### Loading 載入類別
載入.class byte Code。

1. 建立 metadata（Metaspace) 
2. 建立 java.lang.Class（Heap）

### Verification 驗證
檢查class 檔案格式正確

### Preparation 準備
設置static屬性預設值。<br>
把靜態變數預設為原本的值，假設int就是0，String就是null，float就是0.0f。<br>

以下的程式碼，在Preparation階段，把i設為0。<br>
{% highlight java linenos %}
class StaticTest {
  static int i = 300;
}
{% endhighlight %}

### Resolution 解析
在Preparation時，靜態變數是最原本預設值，int就是0，String就是null。<br>

Resolution階段，把靜態變數設為程式碼中的初始值，以下程式碼i原本在Preparation準備階段是0，在Resolution解析階段，設為300。<br>
{% highlight java linenos %}
class StaticTest {
  static int i = 300;
}
{% endhighlight %}

#### 符號 套件名 常數 類別名替換

[Constatn Pool][2]進行套件名、類別名、常數轉換。

### Initialization初始化
執行靜態區塊。
{% highlight java linenos %}
class StaticTest {
  static {
    System.out.println("進到靜態區塊");
    i = 100;
  }
  static int i = 300;
{% endhighlight %}

JVM會將上面的程式碼，把多餘的程式碼去掉或合併，透過Clinit()函式執行優化後的靜態區塊。

優化後:
{% highlight java linenos %}
class StaticTest {
  static {
    System.out.println("進到靜態區塊");
    i = 300;
  }
}
{% endhighlight %}

若是用new建立物件，此階段也會呼叫匿名區塊與建構子。

## 只會建立Class物件，不會建立Object物件
類別載入的動作只會在Heap中，建立Class物件，不會建立Object物件。<br>

呼叫靜態變數，就會載入類別，建立Class物件，以下程式碼不會執行到建構子區塊，不會建立物件。<br>

{% highlight java linenos %}
public class ReflectTest3 {
  public static void main(String[] args) {
    System.out.println(B.num);
  }
}
class B {
  static {
    // 1.
    System.out.println("1.static 區塊");
    // 2.
    num = 300;
  }
  // 3.
  static int num = 100;
  public B() {
    System.out.println("呼叫建構子");
  }
}
{% endhighlight %}
```
1.static 區塊
100
```

另外，執行結果是100，不是300，因為Clinit()，會把以下程式碼合併在一起。<br>
{% highlight java linenos %}
class B {
  static {
    // 1.
    System.out.println("1.static 區塊");
    // 2. 以下二者合併
    num = 300;
    num = 100;
  }
  static int num;
  public B() {
    System.out.println("呼叫建構子");
  }
}
{% endhighlight %}



[1]: {% link _pages/java/metadata.md %}
[2]: {% link _pages/java/metadata.md %}#Constantpool

