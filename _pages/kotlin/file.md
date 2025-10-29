---
title: File
date: 2025-10-29
keywords: kotlin, File
---
## 檔案語法
{% highlight kotlin linenos %}
val file = File("/Users/cici/testc/file_test")
{% endhighlight %}

## 字串與Byte互轉
byte轉成字串，第二個參數是編碼。
{% highlight kotlin linenos %}
val str = String(bytes, Charsets.UTF_8)
{% endhighlight %}

字串轉成byte。
{% highlight kotlin linenos %}
val str = "Nice to see you."
val bytes = str.toByteArray()
{% endhighlight %}

## 小檔案
### 文字讀取
{% highlight kotlin linenos %}
  @Test
  fun test1() {
    val file = File("/Users/cici/testc/file_test")
    println(file.readText())
  }
{% endhighlight %}

### 文字寫入(覆蓋)
{% highlight kotlin linenos %}
  @Test
  fun test3() {
    val file = File("/Users/cici/testc/file_test")
    file.writeText("Hello World!")
    println(file.readText())
  }
{% endhighlight %}

### 文字寫入(未覆蓋)
{% highlight kotlin linenos %}
  @Test
  fun test4() {
    val file = File("/Users/cici/testc/file_test")
    file.appendText("Nice to see you.")
    println(file.readText())
  }
{% endhighlight %}

### byte讀取
輸出檔案大小
{% highlight kotlin linenos %}
  @Test
  fun test2() {
    val file = File("/Users/cici/testc/file_test")
    val bytes = file.readBytes()
    println(bytes.size)
  }
{% endhighlight %}
```
30
```
### byte寫入(覆蓋)
{% highlight kotlin linenos %}
  @Test
  fun test5() {
    val file = File("/Users/cici/testc/file_test")
    file.writeBytes("Nice to see you.".toByteArray())
    println(file.readText())
  }
{% endhighlight %}

### byte寫入(未覆蓋)
{% highlight kotlin linenos %}
  @Test
  fun test5() {
    val file = File("/Users/cici/testc/file_test")
    file.appendBytes("Nice to see you.".toByteArray())
    println(file.readText())
  }
{% endhighlight %}

## use
使用use擴展函式，若物件有實作Closeable，結束時，就可以使用use自動呼叫物件.close()方法。<br>
{% highlight kotlin linenos %}
val file = File("/Users/cici/testc/file_test")
file.inputStream().use { input ->
}
{% endhighlight %}

use原始碼如下:僅截取close()的程式碼
{% highlight kotlin linenos %}
when {
    apiVersionIsAtLeast(1, 1, 0) -> this.closeFinally(exception)
    this == null -> {}
    exception == null -> close()
    else ->
        try {
            close()
        } catch (closeException: Throwable) {
            // cause.addSuppressed(closeException) // ignored here
        }
}
{% endhighlight %}

## inputStream
### read()
一次讀取一個byte，但直接強制轉型成Int。<br>

read()有資料，回傳 0-255。<br>

read()沒資料，檔案結尾，回傳 -1。<br>

`read().also()` 將read()回傳值當作參數，傳入also() 函式中，所以在it是read()回傳值，把回傳值 設給 data變數，並把read()回傳值作為return值。<br>

把read()傳回值，設給data變數。
{% highlight kotlin linenos %}
input.read().also { data = it }
{% endhighlight %}

read()回傳值作為return值，並判斷return值是否為-1檔案結尾。
{% highlight kotlin linenos %}
while (input.read().also { data = it } != -1) { }
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
  @Test
  fun test6() {
    val file = File("/Users/cici/testc/file_test")
    file.inputStream().use { input ->
      var data: Int
      while (input.read().also { data = it } != -1) {
        print("${data} ")
      }
    }
  }
{% endhighlight %}

### read(buffer)
buffer，每次固定讀取多少大小的資料。<br>








### 釋放資源
finally是不管如何都會執行，可在finally中釋放資源。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin22() = runBlocking {
    var br: BufferedReader? = null
    try {
      br = BufferedReader(FileReader("/Users/cici/testc/file_test"))
    } finally {
      br?.close()
    }
  }
{% endhighlight %}



### use
使用use擴展函式，若物件有實作Closeable，結束時，就可以使用use自動呼叫物件.close()方法。<br>
{% highlight kotlin linenos %}
@Test
fun coroutin23() = runBlocking {
  var br = BufferedReader(FileReader("/Users/cici/testc/file_test"))
  br.use {
    var line: String?
    while (true) {
      line = it.readLine() ?: break
      println(line)
    }
  }
}
{% endhighlight %}
```
測試程式

java程式設計
```

use原始碼如下:僅截取close()的程式碼
{% highlight kotlin linenos %}
when {
    apiVersionIsAtLeast(1, 1, 0) -> this.closeFinally(exception)
    this == null -> {}
    exception == null -> close()
    else ->
        try {
            close()
        } catch (closeException: Throwable) {
            // cause.addSuppressed(closeException) // ignored here
        }
}
{% endhighlight %}

讀取檔案無法寫在while()中，以前在java讀取每一行都會寫在while()中，但kotlin不行。<br>
以下程式碼編譯失敗。<br>
{% highlight kotlin linenos %}
var br = BufferedReader(FileReader("/Users/cici/testc/file_test"))
br.use {
  var line: String?
  while ((line = it.readLine()) !== null) {
    println(line)
  }
}
{% endhighlight %}

以下是java的程式碼:
{% highlight java linenos %}
BufferedReader bufferedReader = null;
try {
  bufferedReader = new BufferedReader(new FileReader("/Users/cici/testc/file_test"));
  String line = null;
  while((line = bufferedReader.readLine()) != null) {
    System.out.println(line);
  }
}
{% endhighlight %}