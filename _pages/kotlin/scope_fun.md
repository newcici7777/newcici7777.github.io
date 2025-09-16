---
title: let,run,with,also,apply...
date: 2025-05-14
keywords: kotlin, Scope function, let, run, with, also, apply
---
Prerequisites:

- [Lambda][1]
- [擴展函式][2]

請務必看完擴展函式，再看之後的內容，才容易明白。

## 擴展函式
這些擴展函式，只是為了讓程式碼比較好看。

|擴展函式|Lambda參數|傳回值|
|:----|:----|:---------|
|let  |it  |最後一行決定|
|run  |this|最後一行決定|
|with |this|最後一行決定|
|also |it  |呼叫它的物件|
|apply|this|呼叫它的物件|

## let
```
File("檔名").let()
   ↑
   it
```
- it，為呼叫擴展函式let()的物件，也就是File本身，`.let()`之前物件。
- 傳回值是lambda最後一行。

setWritable() 和 setExecutable() 這些方法的返回值是 Boolean，表示操作是否成功
{% highlight kotlin linenos %}
val var1 = File("/Users/cici/testc/file_test").let {
    it.setWritable(true)
    it.setExecutable(true) //最後一行的結果才是傳回值
}
// 傳回值
println("var1 = $var1")
{% endhighlight %}
```
file1 = true
```

沒有擴展函式之前:
{% highlight kotlin linenos %}
fun main() {
    val file = File("/Users/cici/testc/file_test")
    file.setWritable(true)
    file.setExecutable(true)
}
{% endhighlight %}

### apply
```
File("檔名").apply()
   ↑
   this
```
- this，為呼叫擴展函式apply()的物件，也就是File本身，`.apply()`之前物件。
- 傳回值是this，File本身。
- 可以隱藏this

this未隱藏前。
{% highlight kotlin linenos %}
val file2 = File("/Users/cici/testc/file_test").apply {
    this.setWritable(true)
    this.setExecutable(true)
}
{% endhighlight %}

this隱藏後。
{% highlight kotlin linenos %}
val file2 = File("/Users/cici/testc/file_test").apply {
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

### run
```
File("檔名").run()
   ↑
   this
```
- this，為呼叫擴展函式run()的物件，也就是File本身，`.run()`之前物件。
- 傳回值是lambda最後一行。
- 可以隱藏this

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
```
File("檔名").with()
   ↑
   this
```
- this，為呼叫擴展函式with()的物件，也就是File本身，`.with()`之前物件。
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
```
File("檔名").let()
   ↑
   it
```
- it，為呼叫擴展函式also()的物件，也就是File本身，`.also()`之前物件。
- 傳回值是呼叫它的物件，此處是File。

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

|擴展函式|lambda參數|傳回值|用法|
|:----|:----|:---------|:-----------------|
|let  |it  |最後一行決定|null安全呼叫、修改內容|
|run  |this|最後一行決定|運算、判斷true或false、函式參考|
|with |this|最後一行決定|與run功能相同，但沒有函式參考|
|also |it  |呼叫它的物件|鏈式呼叫（Method chaining）對同一個物件進行一系列的操作|
|apply|this|呼叫它的物件|初始化、config設定|

初始化、config設定
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

對同一個物件進行一系列的操作。
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

計算、判斷true或false
{% highlight kotlin linenos %}
val res = file.run {
    this.readText().contains("Java")
}
println("res = $res")
{% endhighlight %}
```
res = false
```

with與run功能相同
{% highlight kotlin linenos %}
val res2 = with(file) {
    this.readText().contains("Java")
}
println("res2 = $res2")
{% endhighlight %}
```
res2 = false
```

也可作為多行程式碼運算，或資料整理，最後一行回傳最後結果。
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
配合安全呼叫問號?，不是null才呼叫let花括號\{\}中的內容，最後再配合:?貓王運算子，得到null結果時指定預設值。
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

### also與null
配合安全呼叫問號?，若file為空值，後面的also就不會再執行了。
{% highlight kotlin linenos %}
file?.also {
    println("file size = ${it.length()}")
}.also {
    println("file path = ${it.path}")
}.also {
    println("file content= ${it.readText()}")
}
{% endhighlight %}

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
以下為有條件的Lambda
### takeIf
- it，為呼叫Lambda函式的物件。
- 根據條件傳回呼叫它的物件，或null。

判斷是否符合條件，符合條件傳回呼叫它的物件，否則傳回null，takeIf{...}?，後面要加上問號?，因為有可能會為null，是空值類型。

以下範例是做鏈式呼叫，若條件成立，會傳回File物件，然後呼叫readText()，若條件不成立，就直接傳回null，不繼續執行readText。
{% highlight kotlin linenos %}
var result = File("/Users/cici/adbd").takeIf { it.exists() }?.readText()
println(result)
{% endhighlight %}
```
null
```

### takeUnless
- it，為呼叫Lambda函式的物件。
- 根據條件傳回呼叫它的物件，或null。

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


[1]: {% link _pages/kotlin/lambda.md %}
[2]: {% link _pages/kotlin/exten_func.md %}