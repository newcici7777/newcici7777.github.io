---
title: File
date: 2025-10-29
keywords: kotlin, File
---
## 檔案語法
{% highlight kotlin linenos %}
val file = File("/Users/cici/testc/file_test")
{% endhighlight %}

### 取得檔案大小
{% highlight kotlin linenos %}
    val file = File("/Users/cici/testc/file_test")
    println(file.length())
{% endhighlight %}

## 字串與Byte互轉
byte轉成字串，第二個參數是編碼。
{% highlight kotlin linenos %}
val str = String(bytes)
val str = String(bytes, Charsets.UTF_8)
{% endhighlight %}

有開始位置與結束位置。
{% highlight kotlin linenos %}
val str = String(buffer, 0, len)
val str = String(buffer, 0, len, Charsets.UTF_8)
{% endhighlight %}

字串轉成byte。
{% highlight kotlin linenos %}
val str = "Nice to see you."
val bytes = str.toByteArray()
{% endhighlight %}

Int轉成字元、轉成byte。
{% highlight kotlin linenos %}
@Test
fun test8() {
  val data = 97
  println(data.toChar())
  println(data.toByte())
}
{% endhighlight %}
```
a
97
```

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
### read() 回傳值是資料
一次讀取一個byte，但直接強制轉型成Int。<br>

read()有資料，回傳 0-255。<br>

read()沒資料，檔案結尾，回傳 -1。<br>

`read().also()` 將read()回傳值當作參數，傳入also() 函式中，所以it是read()回傳值，把回傳值 設給 data變數，並把read()回傳值作為return值。<br>

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
        print("${data.toChar()} ")
      }
    }
  }
{% endhighlight %}
```
H e l l o   W o r l d ! N i c e   t o   s e e   y o u . N i c e   t o   s e e   y o u . 
```
### read(buffer) 回傳值是讀取到的大小
使用ByteArray()建立暫存空間。<br>

注意！回傳值是讀取到的大小，不是資料！！！<br>

資料存在buffer變數中。<br>

buffer變數為資料暫存的區域，len為每次讀到的大小。<br>

以下buffer的大小為1024 byte。<br>
{% highlight kotlin linenos %}
  @Test
  fun test7() {
    val file = File("/Users/cici/testc/file_test")
    file.inputStream().use { input ->
      val buffer = ByteArray(1024)
      var len: Int
      while (input.read(buffer).also { len = it } != -1) {
        print("${String(buffer, 0, len)} ")
      }
    }
  }
{% endhighlight %}
```
Hello World!Nice to see you.Nice to see you.
```

## outputStream 寫入byte
目前標準作法是outputStream()一定要包在inputStream()，並使用2個use。
{% highlight kotlin linenos %}
val file = File("/Users/cici/testc/file_test")
val file2 = File("/Users/cici/testc/file_test2")
file.inputStream().use { input ->
  file2.outputStream().use { output ->
      // 程式碼要寫這
  }
}
{% endhighlight %}

完整程式碼:
{% highlight kotlin linenos %}
@Test
fun test9() {
  val file = File("/Users/cici/testc/file_test")
  val file2 = File("/Users/cici/testc/file_test2")
  file.inputStream().use { input ->
    file2.outputStream().use { output ->
      val buffer = ByteArray(1024)
      var len: Int
      while (input.read(buffer).also { len = it } != -1) {
        output.write(buffer, 0 , len)
      }
    }
  }
  println(file2.readText())
}
{% endhighlight %}

### copyTo
先前的寫法改成copyTo。
{% highlight kotlin linenos %}
  @Test
  fun test10() {
    val file = File("/Users/cici/testc/file_test")
    val file2 = File("/Users/cici/testc/file_test2")
    file.inputStream().use { input ->
      file2.outputStream().use { output ->
        input.copyTo(output)
      }
    }
    println(file2.readText())
  }
{% endhighlight %}

## bufferReader bufferedWriter
一行行讀取，注意是 `!= null` 不是 `!= -1`。<br>
讀取出來的資料型態是String。<br>
{% highlight kotlin linenos %}
  @Test
  fun test11() {
    val file = File("/Users/cici/testc/file_test")
    file.bufferedReader().use { reader ->
      var line: String
      while (reader.readLine().also { line = it } != null) {
        println("$line ")
      }
    }
  }

{% endhighlight %}

一行行寫入。可以使用newLine()。<br>
{% highlight kotlin linenos %}
  @Test
  fun test12() {
    val file = File("/Users/cici/testc/file_test")
    file.bufferedWriter().use { writer ->
      writer.write("test!")
      writer.newLine()
      writer.write("test2!")
    }
    println(file.readText())
  }
{% endhighlight %}
```
test!
test2!
```

## useLines
可以處理大檔案、自動關閉檔案，把每一行字串轉成Sequence<String>。
{% highlight kotlin linenos %}
val result = file.useLines { lines: Sequence<String> ->

}
{% endhighlight %}

### 讀取每一行文字
{% highlight kotlin linenos %}
@Test
fun test13() {
  val file = File("/Users/cici/testc/file_test")
  file.useLines { lines ->
    lines.forEach { line ->
      println(line)
    }
  }
}
{% endhighlight %}
```
test!
test2!
```

## forEachLine
只能簡單輸出資料。
{% highlight kotlin linenos %}
  @Test
  fun test14() {
    val file = File("/Users/cici/testc/file_test")
    file.forEachLine { line ->
      println(line)
    }
  }
{% endhighlight %}
```
test!
test2!
```

### Java型的寫法(不建議)
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
