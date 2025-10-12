---
title: Java與kotlin 互轉
date: 2025-10-12
keywords: kotlin, 
---
## Java檔案
以下是Java檔案，提供二個方法getData()與getNullData()，其中一個有傳回字串，另一個傳回null。<br>

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public String getData() {
        return "Hello World";
    }
    public String getNullData() {
        return null;
    }
}
{% endhighlight %}

## Kotlin建立Java物件
以下是Kotlin檔案，建立Java物件，不使用new，而是使用()圓括號建立物件。

KotlinTest.kt
{% highlight kotlin linenos %}
fun main() {
    val javaTest = JavaTest()
}
{% endhighlight %}

## Kotlin呼叫Java方法
nullStr會自動轉成String!的型別，代表Java物件傳回的是null。<br>
使用此物件要用問號?，問號代表物件不是null，才可以執行toLowerCase()，否則就不執行，執行時不會有nullpoint Exception。<br>

KotlinTest.kt
{% highlight kotlin linenos %}
fun main() {
    val javaTest = JavaTest()
    println(javaTest.data)
    val nullStr = javaTest.nullData
    nullStr?.toLowerCase()
}
{% endhighlight %}
```
Hello World
```

## javaClass取得Java類型
成員變數intValue是int類型。<br>

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public int intValue = 10;
}
{% endhighlight %}

使用javaClass可以知道成員變數intValue的類型。<br>

KotlinTest.kt
{% highlight kotlin linenos %}
    val intValue = javaTest.intValue
    println(intValue.javaClass)
{% endhighlight %}
```
int
```

## getter setter 存取
不需要使用get成員變數來存取，直接使用成員變數名來存取。<br>

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
{% endhighlight %}

KotlinTest.kt
{% highlight kotlin linenos %}
fun main() {
    val javaTest = JavaTest()
    javaTest.name = "Mary"
    println(javaTest.name)
}
{% endhighlight %}
```
Mary
```

## Java呼叫Kotlin
下方有一個函式名callKotlinFun()。<br>

KotlinTest.kt
{% highlight kotlin linenos %}
fun main() {

}
fun callKotlinFun() = "Kotlin Fun data"
{% endhighlight %}

呼叫方式: 檔名 \+ Kt \+ .函式名() <br>
Java檔案要呼叫KotlinTest<span class="markline">Kt</span>.callKotlinFun()。<br>

KotlinTest.kt
{% highlight java linenos %}
public class JavaTest {
    public static void main(String[] args) {
        System.out.println(KotlinTestKt.callKotlinFun());
    }
}    
{% endhighlight %}
```
Kotlin Fun data
```

### 修改kotlin檔名
語法
```
@file:JvmName("檔名")
```

KotlinTest.kt
{% highlight kotlin linenos %}
@file:JvmName("KotlinTest1")
fun main() {

}
fun callKotlinFun() = "Kotlin Fun data"
{% endhighlight %}

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public static void main(String[] args) {
        System.out.println(KotlinTest1.callKotlinFun());
    }
}
{% endhighlight %}
```
Kotlin Fun data
```

## Java存取Kotlin物件成員變數
KotlinTest.kt中有一個類別Students，成員變數nameList。

KotlinTest.kt
{% highlight kotlin linenos %}
fun main() {
}
class Students {
    val nameList = listOf<String>("Mary", "Bill", "Alex")
}
{% endhighlight %}

使用Kotlin類別的成員變數，要用get、set。

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public static void main(String[] args) {
        Students students = new Students();
        System.out.println(students.getNameList());
    }
}
{% endhighlight %}
```
[Mary, Bill, Alex]
```

### @JvmField
使用@JvmField，就可以不用使用get set存取Kotlin類別的成員變數。

KotlinTest.kt
{% highlight kotlin linenos %}
fun main() {
}
class Students {
    @JvmField
    val nameList = listOf<String>("Mary", "Bill", "Alex")
}
{% endhighlight %}

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public static void main(String[] args) {
        Students students = new Students();
        System.out.println(students.nameList);
    }
}
{% endhighlight %}
```
[Mary, Bill, Alex]
```

### @JvmOverloads
有一個test1()函式，有二個參數，有預設值。<br>

KotlinTest.kt
{% highlight kotlin linenos %}
fun main() {
}
fun test1(arg1:String = "arg1", arg2:String = "arg2") {
    println("arg1 = $arg1, arg2 = $arg2")
}
{% endhighlight %}


在Java中無法只用一個參數，需要用2個參數。<br>

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public static void main(String[] args) {
        KotlinTestKt.test1("test1");
    }
}
{% endhighlight %}

在Kotlin方法上方加上@JvmOverloads，Java就可以使用一個參數來呼叫Kotlin函式。<br>

KotlinTest.kt
{% highlight kotlin linenos %}
@JvmOverloads
fun test1(arg1:String = "arg1", arg2:String = "arg2") {
    println("arg1 = $arg1, arg2 = $arg2")
}
{% endhighlight %}

JavaTest.java
{% highlight java linenos %}
public class JavaTest {
    public static void main(String[] args) {
        KotlinTestKt.test1("test1");
    }
}
{% endhighlight %}
```
arg1 = test1, arg2 = arg2
```