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
1. 會在metaspace建立metadata，裡面包含有類別所有屬性、方法、靜態變數…等等
2. 接著在Heap建立一個Class物件，裡面也有屬性、方法、建構子…等等，但實際都是指向metadata的資料。

![img]({{site.imgurl}}/java/obj_model1.png)

## 載入順序

```
Loading ClassLoader 載入.class
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
### Loading 載入類別
載入.class。

建立 metadata（Metaspace)和 java.lang.Class（Heap）

### Verification
檢查class 檔案格式正確

### Preparation準備
把靜態變數預設為原本的值，假設int就是0，String就是null，float就是0.0f。

### Resolution
在Preparation時，靜態變數是最原本預設值，int就是0，String就是null。

Resolution階段，把靜態變數變成程式碼中設定的值。
{% highlight java linenos %}
class StaticTest {
  static int i = 300;
}
{% endhighlight %}

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

[1]: {% link _pages/java/metadata.md %}
[2]: {% link _pages/java/metadata.md %}#Constantpool

