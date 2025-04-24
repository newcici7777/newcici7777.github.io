---
title: Singleton單例
date: 2025-04-23
keywords: Java, Design patterns, Singleton
---
Prerequisites:

- [靜態只產生一次][1]
- [靜態內部類][2]
- [使用到靜態內部類別才會被建立][3]

[1]: {% link _pages/c/test/test.md %}

Singleton 有很多中文翻譯，如∶ 獨體，單例

什麼是Singleton？Singleton確保一個類別只有一個實體。

## 靜態變數建立Singleton
使用靜態只產生一次的特性，儲存物件，確保一個類別只有一個實體。

步驟如下:
1. 把建構子變成私有
2. 私有靜態變數來存放物件
3. 提供public靜態的方法，傳回物件。

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

測試是否只產生一個物件，測試結果hashCode都是相同，證明不管產生幾次都為相同物件。
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

### 優缺點分析
#### 優點
static方法、static變數、static區塊、static靜態內部類別，是在JVM啟動時，ClassLoader類別載入器就已經先丟進Method area(Metaspace)的靜態資料區中，若靜態變數的類型是物件(如String)，就會把「靜態變數」放在jvm的stack中，而變數指向的「物件」則放入jvm的heap(堆)的區域中。

以上這些動作只做一次，不管產生多少次物件，都只做一次。

#### 缺點
不管有沒有使用到，都會產生一次物件，若沒使用到這個物件就會浪費記憶體位址去存放這個物件。

## Singleton寫在靜態區塊
與以上寫法的優缺點一樣，二者是相同的東西。
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

## 靜態內部類別建立Singleton
{% highlight java linenos %}
class Singleton1 {
  // 1. 把建構子存取權限改成private
  private Singleton1() {}
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
- 靜態只產生一次
- 使用到靜態內部類別才會被建立，要使用的時候才去建這個類別，才不會造成記憶體空間浪費。

## synchronized加上雙重檢查
不使用靜態變數或靜態區塊存放物件，自行建立的好處在於，要使用的時候才去建這個類別。

### 未優化前(不推薦使用)
在方法上加上synchronized，一次只能有一個執行緒進入執行，執行完成後輪下一個執行緒執行此方法，若後面有1000個執行緒要執行這個方法，會造成執行緒排隊等待，執行效能緩慢，以下方法不推薦。
{% highlight java linenos %}
class Singleton1 {
  // 1. 把建構子存取權限改成private
  private Singleton1() {}
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

Singleton1.class是Class類別的物件

getInstance()是靜態方法，鎖的是類別，所以使用synchronized(類別.class)

### 為什麼要檢查二次

為什麼要檢查二次 if (instance == null) ?

想像一下，若有一個執行緒執行完第一個if (instance == null)，然後突然控制權輪到下一個執行緒來使用並建好instance，建完instance後，控制權又輪回前一個執行緒，它才剛執行完if (instance == null)，要開始建立instance，然後就建立第2個instance成功了。

為了不讓人建立第2個物件，所以在同步synchronized區塊內加上第二次 if (instance == null)。

### 加快效率
第3個執行緒只要看到第一個 if (instance == null)，此時instance已經被建立了，就會跳離這個if判斷式，然後就不用再執行synchronized區塊，也就是說synchronized區塊最多只被執行2次。

### volatile
volatile中文是可見性

每個變數都有自己的cache(暫存空間)，有變更不會立刻改變，好幾百個執行緒讀取這個變數，各個執行緒可能會先cache暫存起來，加上volatile，就是不讓各個執行緒把這個變數暫存，永遠重新讀取變數的值。


[1]: {% link _pages/java/static.md %}#靜態只產生一次
[2]: {% link _pages/java/innerclass.md %}#靜態內部類別
[3]: {% link _pages/java/innerclass.md %}#使用到靜態內部類別才會被建立