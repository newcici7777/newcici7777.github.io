---
title: Singleton
date: 2025-04-23
keywords: Java, Design patterns, Singleton
---
Prerequisites:

- [static][1]

本篇文章需要相當多大量知識，了解[類別載入][7]，[metadata][8]，[靜態變數][1]、[靜態區塊][6]、[靜態內部類別][2]、[final][9]、[執行緒][11]、[synchronized][12]，擁有以上的知識，才能了解下面的內容。

Singleton 有很多中文翻譯，如∶ 獨體，單例

什麼是Singleton？Singleton是類別的靜態變數，而這個靜態變數是類別本身的物件。

而類別的變數只會在[類別載入記憶體][7]時，建立一次，確保不會被其它人建立多次。

何時用到Singleton，消耗系統資源的建立的物件，但又頻繁使用，只想只建立一次，不想頻繁的建立。

## 靜態變數建立Singleton
步驟如下:
1. 把建構子變成私有。
2. 私有靜態變數來存放本身類別的物件。
3. 提供public靜態的方法，傳回物件。

為什麼建構子要變成私有，確保只有類別本身才可以呼叫建構子，其它類別都不能呼叫建構子建立物件。

{% highlight java linenos %}
class Singleton1 {
  // 1. 把建構子存取權限改成private
  private Singleton1() {}
  // 2. 私有靜態變數，存放物件
  private static Singleton1 instance = new Singleton1();
  // 3. 提供public static方法，傳回物件
  public static Singleton1 getInstance() {
    return instance;
  }
}
{% endhighlight %}

測試是否只產生一個物件，測試結果[hashCode][10]都是相同，證明不管產生幾次都為相同物件。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Singleton1 singleton1 = Singleton1.getInstance();
    Singleton1 singleton2 = Singleton1.getInstance();
    System.out.println("singleton1 = " + singleton1.hashCode());
    System.out.println("singleton2 = " + singleton2.hashCode());
    boolean result = (singleton1 == singleton2);
    System.out.println("singleton1 == singleton2 = " + result);
  }
}
{% endhighlight %}
```
singleton1 = 821270929
singleton2 = 821270929
singleton1 == singleton2 = true
```

## Singleton寫在靜態區塊
- [靜態區塊][6]
- [靜態只產生一次][1]

靜態變數是在靜態區塊中才建立物件，跟之前的作法沒什麼不同。
{% highlight java linenos %}
class Singleton1 {
  // 1. 把建構子存取權限改成private
  private Singleton1() {}
  private static Singleton1 instance;
  // 2. 寫在靜態區塊
  static {
    instance = new Singleton1();
  }
  // 3. 提供public static方法，傳回物件
  public static Singleton1 getInstance() {
    return instance;
  }
}
{% endhighlight %}

### 優缺點分析
#### 優點
static方法、static變數、static區塊，[ClassLoader類別載入][7]時就已經存放在[metadata][8]。

靜態相關屬性、方法、變數，不管產生多少次物件，都只載入記憶體一次。

#### 缺點
不管有沒有使用到，都會產生一次物件，若沒使用到這個物件就會浪費記憶體位址去存放這個物件。

如果singleton物件要消耗很多執行時間，可能要網路連線、資料庫連線、檔案載入，執行速度就會很慢。

## 靜態內部類別建立Singleton
- [final][9]
- [靜態內部類][2]
- [使用到靜態內部類別才會被建立][3]
- [外部類別存取private內部類別][4]

為了解決先前提到的缺點，使用靜態內部類別，可以做到使用的時候，才會建立。

{% highlight java linenos %}
class Singleton1 {
  // 1. 把建構子存取權限改成private
  private Singleton1() {}
  // 靜態內部類別
  private static class InnerClz {
    // final代表不能再被修改
    private static final Singleton1 INSTANCE = new Singleton1();
  }
  // 2. 提供public static方法，傳回物件
  public static Singleton1 getInstance() {
    return InnerClz.INSTANCE;
  }
}
{% endhighlight %}

### 優點
- 靜態只產生一次物件。
- 使用到靜態內部類別才會被建立，要使用的時候才去建這個類別，才不會造成記憶體空間浪費。

## synchronized加上雙重檢查
不使用靜態內部類別，使用靜態方法 \+ [synchronized][12]，也可以有靜態內部類別的效果(使用的時候才建立物件)。

### 未優化前(不推薦使用)
在靜態方法前加上[synchronized][12]，一次只能有一個[執行緒][11]進入靜態方法執行，執行完成後輪下一個執行緒執行此方法，若後面有1000個執行緒要執行這個方法，會造成執行緒排隊等待，執行效能緩慢，以下方法不推薦。

{% highlight java linenos %}
class Singleton1 {
  // 1. 把建構子存取權限改成private
  private Singleton1() {}
  // 靜態變數
  private static Singleton1 instance;
  
  // 2. 提供public static方法，傳回物件
  public static synchronized Singleton1 getInstance() {
    if (instance == null) {
      instance = new Singleton1();
    }
    return instance;
  }
}
{% endhighlight %}

### 雙重檢查Double-Checked Locking
- [靜態同步方法][12]

好幾百個執行緒進來要取得instance，確保只有一個執行緒建立一個instance。

{% highlight java linenos %}
class Singleton1 {
  // 1. 把建構子存取權限改成private
  private Singleton1() {}
  private static volatile Singleton1 instance;
  // 2. 提供public static方法，傳回物件
  public static Singleton1 getInstance() {
    if (instance == null) {
      synchronized(Singleton1.class) {
        if (instance == null) {
          instance = new Singleton1();
        }
      }
    }
    return instance;
  }
}
{% endhighlight %}

### 為什麼是用synchronized(Singleton1.class)？

Singleton1.class是[Class類別][13]的物件。

getInstance()是類別的靜態方法，[同步鎖][12]，鎖的是類別，所以使用synchronized(類別.class)

### 為什麼要檢查二次

為什麼要檢查二次 if (instance == null) ?

下面這個程式碼只有一次if (instance == null)
{% highlight java linenos %}
class Singleton1 {
  private Singleton1() {}
  private static volatile Singleton1 instance;

  public static Singleton1 getInstance() {
    if (instance == null) {
      synchronized(Singleton1.class) {
          instance = new Singleton1();
      }
    }
    return instance;
  }
}
{% endhighlight %}

想像一下，有一個執行緒A執行完第一個if (instance == null)，然後同步區塊有執行緒B使用中(尚未建立物件)，所以執行緒A在同步區塊外面等待，然後在同步區塊中的執行緒B， 建立好物件instance，離開同步區塊，接著就輪到執行緒A進入同步區塊，接著就建立好第2個instance。

為了不讓人建立第2個物件，所以在同步synchronized區塊內加上第二次 if (instance == null)。

### 加快效率
第3個執行緒只要看到第一個 if (instance == null)，此時instance已經被建立了，就會跳離這個if判斷式，然後就不用再執行synchronized區塊，也就是說synchronized區塊最多只被執行2次。

### volatile
{% highlight java linenos %}
private static volatile Singleton1 instance;
{% endhighlight %}

volatile中文是可見性

每個變數都有自己的cache(暫存空間)，有變更不會立刻改變，好幾百個執行緒讀取這個變數，各個執行緒可能會先cache暫存起來，加上volatile，就是不讓各個執行緒把這個變數暫存，永遠重新讀取變數的值。


[1]: {% link _pages/java/static.md %}#靜態只產生一次
[2]: {% link _pages/java/static_inner.md %}
[3]: {% link _pages/java/static_inner.md %}#使用到靜態內部類別才會被建立
[4]: {% link _pages/java/innerclass.md %}#外部類別可以存取private內部類別與屬性
[5]: {% link _pages/java/static_inner_method.md %}#靜態方法與Singleton
[6]: {% link _pages/java/codeblock.md %}#靜態屬性是建構子
[7]: {% link _pages/java/classloader.md %}
[8]: {% link _pages/java/metadata.md %}
[9]: {% link _pages/java/final.md %}
[10]: {% link _pages/java/java/object.md %}
[11]: {% link _pages/java/java/thread.md %}
[12]: {% link _pages/java/java/sync.md %}
[13]: {% link _pages/java/java/reflect.md %}