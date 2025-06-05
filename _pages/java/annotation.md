---
title: Annotation
date: 2025-06-04
keywords: Java, annotation
---
Annotation，中文稱為標註。

Annotation主要用於檢查。

## 三種Annotation
用 @ 開頭。
- @Override 覆寫父類別方法。
- @Deprecated 類別或方法已棄用。
- @SuppressWarnings 抑制編譯器警告。

## @Override
### @Override錯誤
@Override只能用在方法。

即便不寫@Override，編譯器也會自動判斷是覆寫方法。

寫了@Override，編譯器會去檢查父類別是否有這個方法，如果父類別沒這個方法，是@Override錯誤。

父類別
{% highlight java linenos %}
class Father {
  void method1() {
    System.out.println("Father method1");
  }
  void method2() {
    System.out.println("Father method2");
  }
}
{% endhighlight %}

子類別
{% highlight java linenos %}
class Child extends Father {
  // 此處是@Override錯誤，父類別根本沒有method3()
  @Override
  void method3() {
    this.method2();
    System.out.println(this.hobby);
  }
}
{% endhighlight %}

### @Override原始碼
@interface 代表是一個Annotation。

@Target 只能用在方法 METHOD

{% highlight java linenos %}
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
{% endhighlight %}

## @Deprecated
代表已棄用，但不代表不能用，是不推薦使用。

在類別或方法、屬性上面加上@Deprecated
{% highlight java linenos %}
@Deprecated
class Test1 {
  @Deprecated
  String name;
  @Deprecated
  public void method() {
    
  }
}
{% endhighlight %}

### @Deprecated原始檔
- @Documented 可用於Java Doc文件上。
- @Target 可用於建構子、屬性、區域變數、方法、package、模組、參數、類型

{% highlight java linenos %}
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
public @interface Deprecated {
}
{% endhighlight %}

## @SuppressWarnings 抑制警告
以下紅框的部分就是警告。

![img]({{site.imgurl}}/java/warning.png)

加上以下標註，就沒有警告了。
```
@SuppressWarnings({"all"})
```

![img]({{site.imgurl}}/java/warning2.png)

(\{\"??\"\})裡面可以填寫什麼呢？

- all，抑制所有警告
- boxing，抑制拆箱/裝箱的警告
- cast，抑制強制轉型的警告
- dep-ann，抑制棄用註釋的警告
- deprecation，抑制棄用的警告
- fallthrough，抑制與 switch 陳述式中遺漏 break 相關的警告
- finally，抑制finally 區塊相關的警告
- hiding，抑制與隱藏變數的區域變數相關的警告
- incomplete-switch，抑制與 switch 陳述式 (enum case) 中遺漏項目相關的警告
- javadoc，抑制與 javadoc 相關的警告
- nls，抑制與非 nls 字串文字相關的警告
- null，抑制與空值分析相關的警告
- rawtypes，抑制raw 類型相關的警告
- resource，抑制與使用 Closeable 類型的資源相關的警告
- restriction，抑制與使用不建議或禁止參照相關的警告
- serial，抑制與序列化相關的警告
- static-access，抑制與靜態存取不正確相關的警告
- static-method，抑制與可能宣告為 static 的方法相關的警告
- super，抑制與置換方法相關但不含 super 呼叫的警告
- synthetic-access，抑制與內部類別的存取未最佳化相關的警告
- sync-override，抑制因為置換同步方法而遺漏同步化的警告
- unchecked，抑制沒有檢查的警告
- unqualified-field-access，抑制與欄位存取不合格相關的警告
- unused，抑制沒有使用的警告

把滑鼠移到警告的黃色線，會提示Raw use，因為不知道List裝的是什麼類型。

![img]({{site.imgurl}}/java/warning3.png)

填上rawtypes，警告馬上變成綠色。

![img]({{site.imgurl}}/java/warning4.png)

把滑鼠移到警告的黃色線，會提示unchecked，沒有檢查類型。

![img]({{site.imgurl}}/java/warning5.png)

填上unchecked，警告馬上變成綠色。
![img]({{site.imgurl}}/java/warning6.png)

程式碼
{% highlight java linenos %}
public class Test {
  @SuppressWarnings({"rawtypes", "unchecked"})
  public static void main(String[] args) {
    List list = new ArrayList();
    list.add("adfd");
    list.add("cccc");
  }
}
{% endhighlight %}

SuppressWarnings原始碼
{% highlight java linenos %}
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();  // 是陣列，可以傳入rawtypes,unchecked,unused...等等
}
{% endhighlight %}

## 描述標註的標註
### @Retention 標註的保留
{% highlight java linenos %}
@Retention(RetentionPolicy.SOURCE)
{% endhighlight %}

RetentionPolicy是列舉enum，有下面三種。
1. SOURCE 程式碼編譯完，就會清掉程式碼中的標註。
2. CLASS 編譯完後，程式碼仍保留標註在.class，但執行時，就會清掉。
3. RUNTIME 執行時，標註仍保留在[class物件][1]中，可通過反射找到標註。

下圖中，1與2是標註被清掉的位置。

![img]({{site.imgurl}}/java/annotaion1.png)

### @Target
標註使用的位置，可以是建構子、屬性、區域變數、方法、package、模組、參數、類型TYPE。

TYPE可以為類別、介面、enum列舉...等等。

{% highlight java linenos %}
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
{% endhighlight %}

### @Documented
可在Java Doc顯示出來標註。

[1]: {% link _pages/java/memory_model.md %}
