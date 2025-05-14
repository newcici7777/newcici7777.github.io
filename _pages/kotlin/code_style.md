---
title: Coding Style
date: 2025-05-13
keywords: kotlin, Coding Style
---
[Kotlin Coding Style](https://developer.android.com/kotlin/style-guide?hl=zh-cn)

## 命名
### 駝峰式大小寫
命名步驟如下:
1. 將所有內容（包括首字母縮寫）小寫、將ISO、XML、IP這些常見大寫字母也變小寫。
2. 將每個單字的第一個字元大寫，以產生Pascal 大小寫。
3. 將除第一個單字之外的每個單字的第一個字元大寫。

|未大小寫前|正確|錯誤|
|:----|:----|:----|
|XML Http Request|	XmlHttpRequest|	XMLHTTPRequest|
|new customer ID|	newCustomerId|	newCustomerID|
|inner stopwatch|	innerStopwatch|	innerStopWatch|
|supports IPv6 on iOS|	supportsIpv6OnIos|	supportsIPv6OnIOS|
|YouTube importer|	YouTubeImporter|	YoutubeImporter|

此處命名方式跟C++、Java不同，C++與Java是ISO、XML、IP不用轉小寫。

### 變數
採用camelCase 大小寫形式寫成。物件屬性、參數、可變集合。
{% highlight kotlin linenos %}
val variable = "var"
val nonConstScalar = "non-const"
val mutableCollection: MutableSet
{% endhighlight %}

### 常數
常數大寫，單字用底線分隔。

什麼是常數呢？  
常數是沒有自訂get函式的val屬性，其內容絕對不可變，不可變類型和不可變集合，以及const都為常數。

{% highlight kotlin linenos %}
const val NUMBER = 5
val NAMES = listOf("Alice", "Bob")
val AGES = mapOf("Alice" to 35, "Bob" to 32)
val COMMA_JOINER = Joiner.on(',') // Joiner is immutable
val EMPTY_ARRAY = arrayOf
{% endhighlight %}

### 函式
函式名稱以camelCase 大小寫形式編寫，通常是動詞或動詞片語。例如，sendMessage或stop。
{% highlight kotlin linenos %}
fun sendMessage(name: String) {
    // …
}
{% endhighlight %}

測試函式:允許在測試函式名稱中出現底線，用於分隔名稱的邏輯組成部分。
{% highlight kotlin linenos %}
@Test fun pop_emptyStack() {
    // …
}
{% endhighlight %}

### 類別與介面
類別名稱採用PascalCase 大小寫形式編寫，第一個字母大寫，通常是名詞。例如，Character或ImmutableList。介面名稱也可以是名詞或名詞片語（例如List），但有時也可以是形容詞或形容詞片語（例如Readable）。

{% highlight kotlin linenos %}
class MyClass { }
{% endhighlight %}

{% highlight kotlin linenos %}
class Bar { }
{% endhighlight %}

### 測試類別
測試類別的命名方式是以測試的類別的名稱開頭且以Test結尾。例如，HashTest或HashIntegrationTest。
{% highlight kotlin linenos %}
class HashTest { }
{% endhighlight %}

### 有註解的函式
JetPackCompose中帶有@Composable註解且返回Unit的函式，函式名字首大寫，採用大小寫形式並以名詞形式命名。
{% highlight kotlin linenos %}
@Composable
fun NameTag(name: String) {
    // …
}
{% endhighlight %}

## 縮排
縮排四個空格。

|程式語言|縮排|
|:---:|:---:|
|C++|2格|
|Java|2格|
|Kotlin|4格|

## 換行
### 類別換行
類別的連續成員（屬性、建構子、函式、巢狀類別等）之間

邏輯分組、將程式碼分割為一些邏輯子部分，可換行。

函式與函式之間有換行
{% highlight kotlin linenos %}
fun sum3(vararg num:Int){
    // ...
}

fun sum4(vararg num:Int){
    // ...
}
{% endhighlight %}

### 函式參數換行
當函式簽章不適合放在一行上時，應讓每個參數宣告獨佔一行。以此格式定義的參數應使用單縮排(+4)。右圓括號( )) 和回傳型別獨佔一行，沒有額外的縮排。
{% highlight kotlin linenos %}
fun <T> Iterable<T>.joinToString(
    separator: CharSequence = ", ",
    prefix: CharSequence = "",
    postfix: CharSequence = ""
): String {
    // …
}
{% endhighlight %}

### 變數換行
當屬性初始化式不適合放在一行時，應在等號( =) 後面換行，並進行縮排。
{% highlight kotlin linenos %}
private val defaultCharset: Charset? =
    EncodingRegistry.getInstance().getDefaultCharsetForPropertiesFiles(file)
{% endhighlight %}

### get set換行
宣告get和/或set函式的屬性應讓每個函式獨佔一行，並使用正常的縮排(+4)。對它們進行格式設定時，使用的規則與函式相同。
{% highlight kotlin linenos %}
var directory: File? = null
    set(value) {
        // …
    }
{% endhighlight %}

只讀屬性可以使用適合放在一行的較短語法。
{% highlight kotlin linenos %}
val defaultExtension: String get() = "kt"
{% endhighlight %}

## if、for、when、do、while空格
if後面有空格，左大括號{前面有空格，else前後與大括號之間都有空格。

{% highlight kotlin linenos %}
// WRONG!
if (list.isEmpty()){
}

// Okay
if (list.isEmpty()) {
}

// WRONG!
}else {
}

// Okay
} else {
}

// WRONG!
for(i in 0..1) {
}

// Okay
for (i in 0..1) {
}
{% endhighlight %}

## 運算式空格
{% highlight kotlin linenos %}
// WRONG!
val two = 1+1

// Okay
val two = 1 + 1
{% endhighlight %}

## Lambda空格
箭頭 -> 前後都有空格
{% highlight kotlin linenos %}
// WRONG!
ints.map { value->value.toString() }

// Okay
ints.map { value -> value.toString() }
{% endhighlight %}

## 變數、參數類型冒號
注意變數後面沒有空格，冒號後面一個空格，再來是類型。
{% highlight kotlin linenos %}
val str: String = "Hello"
{% endhighlight %}

函式參數，注意d變數後面沒有空格，冒號後面一個空格，再來是類型。
{% highlight kotlin linenos %}
fun printDouble(d: Double) {
    println(d)
}
{% endhighlight %}

## 繼承、介面、泛型冒號
繼承冒號(:)前後要空格
{% highlight kotlin linenos %}
// WRONG!
class Foo: Runnable

// Okay
class Foo : Runnable
{% endhighlight %}

泛型T的繼承，繼承冒號(:)前後要空格

注意T : Comparable
{% highlight kotlin linenos %}
// WRONG
fun <T: Comparable> max(a: T, b: T)

// Okay
// 注意T : Comparable
fun <T : Comparable> max(a: T, b: T)
{% endhighlight %}

{% highlight kotlin linenos %}
// WRONG
fun <T> max(a: T, b: T) where T: Comparable<T>
{% endhighlight %}

{% highlight kotlin linenos %}
// Okay
// 注意T : Comparable
fun <T> max(a: T, b: T) where T : Comparable<T>
{% endhighlight %}

## 逗號後面空格
逗號後面要一個空白，前面不用空白
{% highlight kotlin linenos %}
// WRONG!
val oneAndTwo = listOf(1,2)

// Okay
val oneAndTwo = listOf(1, 2)
{% endhighlight %}

## 不用空格
::前後沒有空格
{% highlight kotlin linenos %}
// WRONG!
val toString = Any :: toString

// Okay
val toString = Any::toString
{% endhighlight %}

範圍運算子(..)
{% highlight kotlin linenos %}
// WRONG
for (i in 1 .. 4) {
  print(i)
}

// Okay
for (i in 1..4) {
  print(i)
}
{% endhighlight %}

## if、for、when、do、while大括號
if、for、when分支、do和while語句及表達式都需要大括號，即使主體為空或僅包含一個語句也是如此。
{% highlight kotlin linenos %}
if (string.isEmpty())
    return  // WRONG!

if (string.isEmpty()) {
    return  // Okay
}

if (string.isEmpty()) return  // WRONG
else doLotsOfProcessingOn(string, otherParametersHere)

if (string.isEmpty()) {
    return  // Okay
} else {
    doLotsOfProcessingOn(string, otherParametersHere)
}
{% endhighlight %}

## 大括號{}換行
{% highlight kotlin linenos %}
try {
    doSomething()
} catch (e: Exception) {} // WRONG!

try {
    doSomething()
} catch (e: Exception) {
} // Okay
{% endhighlight %}

列舉的大括號，可以不用換行。
{% highlight kotlin linenos %}
enum class Answer { YES, NO, MAYBE }
{% endhighlight %}

## 省略{}大括號
if/else條件語句只有一行才能省略大括號。

{% highlight kotlin linenos %}
val value = if (string.isEmpty()) 0 else 1  // Okay
{% endhighlight %}

超過一行，還是要有大括號。

錯誤範例
{% highlight kotlin linenos %}
val value = if (string.isEmpty())  // WRONG!
    0
else
    1
{% endhighlight %}

對的範例
{% highlight kotlin linenos %}
val value = if (string.isEmpty()) { // Okay
    0
} else {
    1
}
{% endhighlight %}

## 省略類型
變數、函式傳回值有初始值可以推斷類型，就可以省略類型。

省略變數類型
{% highlight kotlin linenos %}
val str: String = "Hello"
// becomes
val str = "Hello"
{% endhighlight %}

省略函式傳回類型
{% highlight kotlin linenos %}
override fun toString(): String = "Hey"
// becomes
override fun toString() = "Hey"
{% endhighlight %}

省略變數類型
{% highlight kotlin linenos %}
private val ICON: Icon = IconLoader.getIcon("/icons/kotlin.png")
// becomes
private val ICON = IconLoader.getIcon("/icons/kotlin.png")
{% endhighlight %}

## 列舉
將枚舉中的常數放在單獨的行上時，它們之間不需要空白行，但它們定義主體時除外。
{% highlight kotlin linenos %}
enum class Answer {
    YES,
    NO,

    MAYBE {
        override fun toString() = """¯\_(ツ)_/¯"""
    }
}
{% endhighlight %}