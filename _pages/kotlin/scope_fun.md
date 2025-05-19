---
title: let,run,with,also,apply...
date: 2025-05-14
keywords: kotlin, Scope function, let, run, with, also, apply
---
Prerequisites:

- [null][1]
- [Lambda][2]

## Lambda參數與傳回值

### Lambda參數與傳回值表格

|函式  |Lambda參數|傳回值|
|let  |it  |最後一行決定|
|run  |this|最後一行決定|
|with |this|最後一行決定|
|also |it  |呼叫它的物件|
|apply|this|呼叫它的物件|

### let
- 會把呼叫它的物件代入，用it
- 傳回值是lambda最後一行。

setWritable() 和 setExecutable() 這些方法的返回值是 Boolean，表示操作是否成功

{% highlight kotlin linenos %}
val var1 = File("/Users/cici/testc/file_test").let {
    it.setWritable(true)
    it.setWritable(true)
    it.setExecutable(true) //最後一行的結果才是傳回值
}
println("var1 = $var1")
println(var1::class)
{% endhighlight %}
```
file1 = true
boolean (Kotlin reflection is not available)
```

### run
- 會把呼叫它的物件代入，用this
- 傳回值是lambda最後一行。

{% highlight kotlin linenos %}
val var3 = File("/Users/cici/testc/file_test").run {
    setWritable(true)
    setWritable(true)
    setExecutable(true) //最後一行的結果才是傳回值
}
println("var3 = $var3")
println(var3::class)
{% endhighlight %}
```
var3 = true
boolean (Kotlin reflection is not available)
```
run可以呼叫函式參考

### with
- 會把呼叫它的物件代入，用this
- 傳回值是lambda最後一行。
- 使用參數的方式代入，with(參數) {}

{% highlight kotlin linenos %}
val var5 = with(File("/Users/cici/testc/file_test")) {
    setWritable(true)
    setWritable(true)
    setExecutable(true) //最後一行的結果才是傳回值
}
println("var5 = $var5")
println(var5::class)
{% endhighlight %}
```
var5 = true
boolean (Kotlin reflection is not available)
```
### also
- 會把呼叫它的物件代入，用it
- 但傳回值是呼叫它的物件。
{% highlight kotlin linenos %}
val file2 = File("/Users/cici/testc/file_test").apply {
    setWritable(true)
    setWritable(true)
    setExecutable(true)
}
println("file2 = $file2")
println(file2::class)
{% endhighlight %}
```
file2 = /Users/cici/testc/file_test
class java.io.File (Kotlin reflection is not available)
```

### apply
- 會把呼叫它的物件代入，用this
- 但傳回值是呼叫它的物件。
{% highlight kotlin linenos %}
val file4 = File("/Users/cici/testc/file_test").also {
    it.setWritable(true)
    it.setWritable(true)
    it.setExecutable(true)
}
println("file4 = $file4")
println(file4::class)
{% endhighlight %}
```
file4 = /Users/cici/testc/file_test
class java.io.File (Kotlin reflection is not available)
```

## 用法

|函式  |Lambda參數|傳回值|用法|
|let  |it  |最後一行決定|null安全呼叫、修改內容|
|run  |this|最後一行決定|運算、建議傳回true或false、函式參考|
|with |this|最後一行決定|計算、轉換、取得資訊|
|also |it  |呼叫它的物件|鏈式呼叫（Method chaining）|
|apply|this|呼叫它的物件|初始化物件|

初始化
{% highlight kotlin linenos %}
val file = File("/Users/cici/testc/file_test").apply {
    setWritable(true)
    setReadable(true)
}
{% endhighlight %}

取出資料
{% highlight kotlin linenos %}
val lines = file.let {
    it.readLines()
}
{% endhighlight %}

鏈式呼叫
{% highlight kotlin linenos %}
file.also {
    println("file size = ${it.length()}")
}.also {
    println("file path = ${it.path}")
}.also {
    println("file content= ${it.readText()}")
}
{% endhighlight %}
```
file size = 30
file path = /Users/cici/testc/file_test
file content= 測試程式

java程式設計
```

計算、判斷
{% highlight kotlin linenos %}
val res = file.run {
    this.readText().contains("Java")
}
println("res = $res")
{% endhighlight %}
```
res = false
```

取得檔案資訊
{% highlight kotlin linenos %}
val fileInfo = with(file) {
    "file name = ${name}  length = ${length()} byte."
}
println(fileInfo)
{% endhighlight %}
```
file name = file_test  length = 30 byte.
```

### let與null
配合安全呼叫問號?，不是null才呼叫let花括號\{\}中的內容，最後再配合:?貓王運算符號，得到null結果時指定預設值。
{% highlight kotlin linenos %}
fun main() {
    // String類型後面加上問號?代表可以是null
    var msg: String? = null;
    println("${showMsg(msg)}")
    println("=================")
    msg = "Hello World!"
    println("${showMsg(msg)}")
}
fun showMsg(arg1: String?) : String {
    // 若為null，就不執行let{}中的內容，顯示預設值null data
    return arg1?.let {
        "Your msg is $arg1"
    } ?: "null data"
}
{% endhighlight %}
```
null data
=================
Your msg is Hello World!
```
### run與函式參考
鏈式呼叫與函式參考二者結合，都會把Lambda最後一行，傳到下一個函式中，最後把showMsg2()的結果印出。
{% highlight kotlin linenos %}
fun main() {
    "abcdefegdddffdfd"
        .run(::isLong)
        .run(::showMsg2)
        .run(::println)
}

fun isLong(str: String): Boolean = str.length > 10

fun showMsg2(isLong: Boolean): String {
    return if (isLong) {
        "String is too long"
    } else {
        "String is not too long"
    }
}
{% endhighlight %}
```
String is too long
```

## 其它
### takeIf
- 會把呼叫它的物件代入，用it
- 傳回值是lambda最後一行。

判斷是否符合條件，符合條件傳回呼叫它的物件，否則傳回null，takeIf{...}?，後面要加上問號?，因為有可能會為null，是可空類型。

以下範例是做鏈式呼叫，若條件成立，會傳回File物件，然後呼叫readText()，若條件不成立，就直接傳回null，不繼續執行readText。
{% highlight kotlin linenos %}
var result = File("/Users/cici/adbd").takeIf { it.exists() }?.readText()
println(result)
{% endhighlight %}
```
null
```

### takeUnless
- 會把呼叫它的物件代入，用it
- 傳回值是lambda最後一行。

與takeIf相反，不符合條件，才會進入let{}中。
{% highlight kotlin linenos %}
var result2 = File("/Users/cici/adbd").takeUnless { it.exists() }.let {
    "file is not exist."
}
println("result = $result2")
{% endhighlight %}
```
result = file is not exist.
```

[1]: {% link _pages/kotlin/null.md %}
[2]: {% link _pages/kotlin/lambda.md %}