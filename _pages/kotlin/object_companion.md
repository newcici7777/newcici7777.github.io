---
title: object與companion object
date: 2025-06-03
keywords: kotlin, object, singleton, companion object
---
Prerequisites:

- [Singleton][1]
- [靜態內部類別][2]

以下的內容比較難明白，需要配合對映的Java程式碼才能明白。<br>

## object 類別
這裡的object，指的是就只有一個物件，不管被建立幾次，只會被建立一次。

[Singleton][1]的概念。

語法
```
object 類別名 {

}
```

使用方法
```
類別名.方法()
類別名.屬性名
```

建立object物件
{% highlight kotlin linenos %}
object Single {
    val name:String = "Single"
    fun method1() {
        println("method1")
    }
    fun method2() {
        println("method2")
    }
}
{% endhighlight %}

測試
{% highlight kotlin linenos %}
fun main() {
    Single.method1()
    Single.method2()
    println(Single.name)
    println(Single.hashCode())
    println(Single.hashCode())
    println(Single.hashCode())
}
{% endhighlight %}
```
method1
method2
Single
1450495309
1450495309
1450495309
```

hashCode如同物件身份證，代表印出三次Single物件，hashCode都相同，代表同一個物件。

Kotlin程式碼
{% highlight kotlin linenos %}
object Single {
    val name: String = "Single"
    fun method1() {
        println("method1")
    }
}
{% endhighlight %}

對映的Java程式碼
{% highlight kotlin linenos %}
public final class Single {
    // 唯一的單例實例
    public static final Single INSTANCE;

    // 屬性對應
    private final String name = "Single";

    // 初始化區塊
    static {
        INSTANCE = new Single();
    }

    // 私有建構子，防止外部建立實例
    private Single() { }

    // Kotlin 的 val name 對應成 getter
    public String getName() {
        return name;
    }

    // Kotlin 的 method1 對應成普通實例方法
    public void method1() {
        System.out.println("method1");
    }
}
{% endhighlight %}

## companion object
這邊不建議解釋為object物件，而是類別的靜態屬性與靜態方法的放置區塊。<br>

把類別的靜態屬性與靜態方法，全放在`companion object {}`大括號之中。<br>
``` 
companion object {
    類別靜態屬性
    類別靜態方法
}
```
Kotlin 寫法。
{% highlight kotlin linenos %}
class Test {
    init {
        println("建立物件")
    }

    companion object {
        init {
            println("類別載入")
        }

        fun staticMethod1() {
            println("靜態方法")
        }
    }
}
{% endhighlight %}

對映Java的寫法。
{% highlight java linenos %}
public class Test {
    {
        System.out.println("建立物件");
    }

    static {
        System.out.println("類別載入");
    }

    public static void staticMethod1() {
        System.out.println("靜態方法");
    }
}
{% endhighlight %}

由上方可以發現，`static {}` 靜態匿名區塊，變成`companion object {}`中的`init {}`，證明這個init是「類別載入」的靜態匿名區塊。<br>
{% highlight kotlin linenos %}
class Test {
    companion object {
        init {
            println("類別載入時執行")
        }
    }
}
{% endhighlight %}

而以下這個init是「物件」建立的時候，才會呼叫。
{% highlight kotlin linenos %}
class Test {
    init {
        println("建立物件")
    }
}
{% endhighlight %}

也就是說kotlin沒有static，但類別的靜態屬性、靜態方法一行放在`companion object {}` 中。
```
class 類別 {
    companion object {
        靜態屬性
        靜態方法
    }
}
```

測試
{% highlight kotlin linenos %}
class Test {
    init {
        println("建立物件")
    }
    companion object {
        init {
            println("載入類別")
        }
        // 靜態屬性
        val info: String = "info"

        // 靜態方法
        fun load() {
            println("載入資訊...")
        }
    }
}
fun main() {
    Test.load()
    println(Test.info)
}
{% endhighlight %}
```
載入類別
載入資訊...
info
```

注意！以上的執行結果，完全沒執行「建立物件」這行。

只能透過類別名，才能呼叫companion object中的靜態屬性與靜態方法。
```
類別名.靜態方法
類別名.靜態屬性
```

Kotlin 沒有 static {}。<br>
但是 Kotlin 有兩種替代方式：

|Java 的靜態概念|  Kotlin 對應方式|
|:----------|:--------------|
|static {} 區塊|在 companion object 裡的 init|
|static 方法|在 companion object 裡定義函式|
|static 欄位|在 companion object 裡定義變數|

以下會編譯錯誤，不能透過物件去呼叫靜態屬性與靜態方法。<br>
{% highlight kotlin linenos %}
fun main() {
    val obj = Test()
    // 以下編譯錯誤
    obj.info
    obj.load()
}
{% endhighlight %}

透過類別名，才能呼叫靜態方法與靜態屬性。
{% highlight kotlin linenos %}
fun main() {
    Test.load()
    println(Test.info)
}
{% endhighlight %}
```
載入類別
載入資訊...
info
```

只有建立物件的時候，才會輸出「建立物件」。
{% highlight kotlin linenos %}
class Test {
    init {
        println("建立物件")
    }
    companion object {
        init {
            println("載入類別")
        }
        // 靜態屬性
        val info: String = "info"

        // 靜態方法
        fun load() {
            println("載入資訊...")
        }
    }
}

fun main() {
    val obj = Test()
}
{% endhighlight %}
```
載入類別
建立物件
```

|功能/階段|   Java|    Kotlin|  執行時機|
|:--------|:-------|:---------|:----------|
|實例初始化（每次 new）|{}（匿名區塊）|init {} |每次建立物件時|
|類別載入時（一次）|static {} |companion object { init { } }|第一次使用類別成員時|
|靜態方法|static void ...()|companion object { fun ...() }|直接用類別呼叫|
|建構子 |類別名()|constructor()或主建構子 |每次建立物件時|

## companion object 委托by
以下程式碼，ConsoleLogger實作 interface Logger。<br>
Service的隱藏靜態內部類別(companion)也要實作 interface Logger。<br>
但是怎麼實作，全交由ConsoleLogger實作。<br>

kotlin 程式碼
{% highlight kotlin linenos %}
interface Logger {
    fun log(msg: String)
}

object ConsoleLogger : Logger {
    override fun log(msg: String) = println("Log: $msg")
}

class Service {
    // 隱藏的靜態內部類別
    companion object : Logger by ConsoleLogger
}

fun main() {
    Service.log("Start service")  // Log: Start service
}
{% endhighlight %}

對映的Java程式碼
{% highlight java linenos %}
public interface Logger {
    void log(String msg);
}

public final class ConsoleLogger implements Logger {
    public static final ConsoleLogger INSTANCE;

    static {
        INSTANCE = new ConsoleLogger();
    }

    private ConsoleLogger() {}

    @Override
    public void log(String msg) {
        System.out.println("Log: " + msg);
    }
}
{% endhighlight %}

靜態內部類別的log方法，呼叫的是ConsoleLogger的log方法。
{% highlight java linenos %}
public final class Service {
    // Companion object 對應成靜態內部類別
    public static final class Companion implements Logger {
        // Kotlin 自動生成的單例
        public static final Companion INSTANCE;

        static {
            INSTANCE = new Companion();
        }

        private Companion() {}

        // 「by ConsoleLogger」表示所有 log() 呼叫都轉交給 ConsoleLogger.INSTANCE
        @Override
        public void log(String msg) {
            ConsoleLogger.INSTANCE.log(msg);
        }
    }
}
{% endhighlight %}

[1]: {% link _pages/design_pattern/singleton.md %}
[2]: {% link _pages/java/static_inner.md %}