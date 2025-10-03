---
title: cancel
date: 2025-09-22
keywords: kotlin, Coroutine cancel
---
## 可以被取消的suspend函式
delay()、withContext()都可以被協程取消。

## job取消協程
以下只會執行list[0]的job，因為運行1.1秒後，所有子協程全被取消。<br>
cancel()是取消協程。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin14() = runBlocking {
    val job = Job()
    val list = listOf(
      launch(job) {
        delay(1000)
        println("list[0] finish")
      },
      launch(job) {
        delay(2000)
        println("list[1] finish")
      })
    job.start()
    // 1.1秒後，取消所有子協程
    delay(1100)
    job.cancel()
    list.forEach { it.join() }
    println("all children finish.")
  }
{% endhighlight %}
```
list[0] finish
all children finish.
````

### join 與 cancel
以下的程式碼child.cancel()不會立刻馬上取消，而join會轉變成「等待」取消完成，確保child協程「執行完畢」。<br>

以下不會有任何執行結果，因為0.1秒(100ms)，就把child.cancel()。<br>
{% highlight kotlin linenos %}
  fun coroutin08() = runBlocking {
    val job = Job()
    val child = launch(job) {
        delay(1000)
        println("child finish")
    }
    // 暫停0.1秒
    delay(100)
    // 取消協程
    child.cancel()
    // runBlocking等待完成取消
    child.join()
  }
{% endhighlight %}

如果只有cancel，協程正在清理資料，但runBlocking執行完了，就退出了。
```
job.cancel()
```

需要二者一起搭配。
{% highlight kotlin linenos %}
    // 取消協程
    job.cancel()
    // 等待
    job.join()
{% endhighlight %}

也可以使用cancelAndJoin()取代。
{% highlight kotlin linenos %}
job.cancelAndJoin()
{% endhighlight %}

### delay()
delay() 是一個可取消的掛起函數，當協程被取消時，delay()會拋出 CancellationException。<br>

但kotlin的CancellationException是被忽略，除非有try{...}catch{...}，才能補捉到CancellationException。<br>

{% highlight kotlin linenos %}
  @Test
  fun coroutin08() = runBlocking {
    val job = Job()
    val child = launch(job) {
      try {
        delay(1000)
        println("child finish")
      }catch (e: Exception) {
        e.printStackTrace()
      }
    }
    // 暫停0.1秒
    delay(100)
    // 取消協程
    child.cancel()
    // runBlocking等待完成取消
    child.join()
  }
{% endhighlight %}
```
kotlinx.coroutines.JobCancellationException: StandaloneCoroutine was cancelled; job="coroutine#3":StandaloneCoroutine{Cancelling}@ff684e1
```

## 作用域Scope 取消
下面的程式碼是，GlobalScope獨立作用域的取消。<br>
取消協程不會「顯示」任何exception，但實際上會拋出CancellationException。<br>

### 取消作用域Scope
作用域scope.cancel()，會直接把相同作用域的協程取消。<br>
{% highlight kotlin linenos %}
@Test
fun coroutin19() = runBlocking {
  val scope = CoroutineScope(Dispatchers.Default)
  val job1 = scope.launch {
    delay(1000)
    println("job1 finish")
  }
  val job2 = scope.launch {
    delay(1000)
    println("job2 finish")
  }
  delay(500)
  scope.cancel()
  job1.join()
  job2.join()
}
{% endhighlight %}

### 取消子協程
以下job2仍會執行，因為只有取消job1。
{% highlight kotlin linenos %}
@Test
fun coroutin19() = runBlocking {
  val scope = CoroutineScope(Dispatchers.Default)
  val job1 = scope.launch {
    delay(1000)
    println("job1 finish")
  }
  val job2 = scope.launch {
    delay(1000)
    println("job2 finish")
  }
  delay(500)
  // 只有取消job1
  job1.cancel()
  job1.join()
  job2.join()
}
{% endhighlight %}
```
job2 finish
```
## try catch CancellationException
cancel()取消時會有CancellationException，但不會在終端機輸出，因為對kotlin來說，這是正常的Exception。<br>

加上try{}... catch{}就可以抓出CancellationException。<br>

可以在cancel()傳入自訂例外的名稱。<br>
```
job1.cancel(CancellationException("自訂取消Exception"))
```

job1.join()變成<span class="markline">等待取消</span>。<br>
{% highlight kotlin linenos %}
  fun coroutin08() = runBlocking {
    val job1 = GlobalScope.launch {
      try {
        delay(1000)
        println("job1")
      }catch (e: Exception) {
        e.printStackTrace()
      }
    }
    delay(100)
    job1.cancel(CancellationException("自訂取消Exception"))
    job1.join()
  }
{% endhighlight %}
```
java.util.concurrent.CancellationException: 自訂取消Exception
  at com.example.coroutine.Test01$coroutin08$1.invokeSuspend(Test01.kt:105)
```

## finally
### 取消但一定會執行finally
不管有沒有被取消，都一定會執行finally{}。<br>

下面程式碼，取消job1，job2沒取消，job1不會輸出"job1 finish"，但取消時會輸出"job1 finally"。<br>
{% highlight kotlin linenos %}
@Test
  fun coroutin19() = runBlocking {
    val scope = CoroutineScope(Dispatchers.Default)
    val job1 = scope.launch {
      try {
        delay(1000)
        println("job1 finish")
      } finally {
        println("job1 finally")
      }
    }
    val job2 = scope.launch {
      try {
        delay(1000)
        println("job2 finish")
      } finally {
        println("job2 finally")
      }
    }
    delay(500)
    job1.cancel()
    job1.join()
    job2.join()
  }
{% endhighlight %}
```
job1 finally
job2 finish
job2 finally
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

### withContext(NonCancellable)
被cancel的協程中，在finally有suspend函式，delay()是suspend函式，不會執行。<br>
以下child1被取消，不會印出「finally 2」。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin24() = runBlocking {
    val parent = Job()
    val child1 = launch(parent) {
      try {
        delay(1000)
        println("child1 finish")
      } finally {
        println("finally 1")
        delay(100)
        println("finally 2")
      }
    }
    val child2 = launch(parent) {
      delay(1000)
      println("child2 finish")
    }
    delay(500)
    child1.cancel()
    child1.join()
    child2.join()
  }
{% endhighlight %}
```
finally 1
child2 finish
```

改用withContext(NonCancellable)包住suspend函式就可以，系統會執行完withContext後才會取消完成。
{% highlight kotlin linenos %}
  @Test
  fun coroutin24() = runBlocking {
    val parent = Job()
    val child1 = launch(parent) {
      try {
        delay(1000)
        println("child1 finish")
      } finally {
        withContext(NonCancellable) {
          println("finally 1")
          delay(100)
          println("finally 2")
        }
      }
    }
    val child2 = launch(parent) {
      delay(1000)
      println("child2 finish")
    }
    delay(500)
    child1.cancel()
    child1.join()
    child2.join()
  }
{% endhighlight %}
```
finally 1
finally 2
child2 finish
```

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

## Cpu運算無法被取消
以下程式碼調度器要用Dispatchers.Default，否則無法取消，Default是屬於CPU運算。<br>

照理說，delay 0.1秒後，job1要被取消，但一直執行，i印到9。<br>
{% highlight kotlin linenos %}
  fun coroutin21() = runBlocking {
    val job1 = launch(Dispatchers.Default) {
      var nexTime = System.currentTimeMillis()
      var i = 0
      try {
        while (i < 10) {
          if (System.currentTimeMillis() >= nexTime) {
            println("i = $i isActive = $isActive")
            i++
            // 每0.5秒循環一次
            nexTime += 500
          }
        }
      } catch (e: Exception) {
        e.printStackTrace()
      }
    }
    delay(100)
    job1.cancel()
    job1.join()
  }
{% endhighlight %}
```
i = 0 isActive = true
i = 1 isActive = false
i = 2 isActive = false
i = 3 isActive = false
i = 4 isActive = false
i = 5 isActive = false
i = 6 isActive = false
i = 7 isActive = false
i = 8 isActive = false
i = 9 isActive = false
```
### Job Cancel狀態
由下表可以知道Cancelling 取消中、Cancelled 取消完成、Completed 完成中，isActive都是false的狀態，所以可以利用isActive來判斷是否在取消中、取消完成。

|狀態|isActive|isCompleted|isCancelled|
|:-------|:---:|:---:|:---:|
|New 建立|false|false|false|
|Active 執行中|true|false|	false|
|Completing 完成中|true|false|false|
|Cancelling 取消中|false|false|true|
|Cancelled 取消完成|false|true|true|
|Completed 完成|false|true|false|

下圖中，Completed完成，isCancelled是false。<br>

取消中跟取消完成，isCancelled是false<br>

取消中(Cancelling)、取消完成(Cancelled)、完成(Completed)，三種狀態，isActive都是false。<br>

會造成取消除了使用cancel()，協程拋出非正常Exception(排除CancellationException)，都會進入到取消中(Cancelling)的狀態。<br>

![img]({{site.imgurl}}/kotlin/job_cancel.png)<br>

### isActive判斷子協程是否被取消
isActive會傳回是否在取消。<br>
若協程被取消，會傳回false。<br>
以下程式3秒後，取消父親為job的所有子協程。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin16() = runBlocking {
    val job = Job()
    val list = listOf(
      launch(job) {
        // isActive會傳回協程是否正在運行中
        while (isActive) {
          println("list[0] runing")
          // 暫停1秒
          delay(1000)
        }
      },
      launch(job) {
        // isActive會傳回協程是否正在運行中
        while (isActive) {
          println("list[1] runing")
          // 暫停1秒
          delay(1000)
        }
      })
    job.start()
    // 3秒後，取消父親為job的所有子協程。
    delay(3000)
    job.cancel()
    list.forEach { it.join() }
    println("子協程全被取消")
  }
{% endhighlight %}
```
list[0] runing
list[1] runing
list[0] runing
list[1] runing
list[0] runing
list[1] runing
子協程全被取消
```

### isActive
加上isActive判斷Job的狀態是否在取消中，若在取消中就不執行。<br>

執行結果只印出i = 0，不會一直印出。<br>

加上try ... catch ... 補捉CancellationException的例外。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin21() = runBlocking {
    val job1 = launch(Dispatchers.Default) {
      var nexTime = System.currentTimeMillis()
      var i = 0
      try {
        while (i < 10 && isActive) {
          if (System.currentTimeMillis() >= nexTime) {
            println("i = $i isActive = $isActive")
            i++
            // 每0.5秒循環一次
            nexTime += 500
          }
        }
      } catch (e: CancellationException) {
        println("補捉到CancellationException Exception")
      }
    }
    delay(100)
    job1.cancel()
    job1.join()
  }
{% endhighlight %}
```
i = 0 isActive = true
補捉到CancellationException Exception
```

### ensureActive()
ensureActive()原始碼也是使用isActive，判斷Job狀態是不是取消中或取消完成。<br>
{% highlight kotlin linenos %}
public fun Job.ensureActive(): Unit {
    if (!isActive) throw getCancellationException()
}
{% endhighlight %}

ensureActive()會拋出JobCancellationException。
{% highlight kotlin linenos %}
public fun getCancellationException(): CancellationException
{% endhighlight %}

加上try ... catch ... 補捉CancellationException的例外。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin21() = runBlocking {
    val job1 = launch(Dispatchers.Default) {
      var nexTime = System.currentTimeMillis()
      var i = 0
      try {
        while (i < 10) {
          ensureActive()
          if (System.currentTimeMillis() >= nexTime) {
            println("i = $i isActive = $isActive")
            i++
            nexTime += 500
          }
        }
      } catch (e: CancellationException) {
        println("補捉到CancellationException Exception")
      }
    }
    delay(100)
    job1.cancel()
    job1.join()
  }
{% endhighlight %}
```
i = 0 isActive = true
補捉到CancellationException Exception
```
### yield
yield()判斷Job狀態是不是取消中或取消完成，密集計算會佔用cpu資源，yield會讓出部分cpu資源給其它的Job使用，不會獨佔Cpu資源，讓出「部分」cpu資源，還是會把密集計算的程式碼完成。<br>
{% highlight kotlin linenos %}
  fun coroutin21() = runBlocking {
    val job1 = launch(Dispatchers.Default) {
      var nexTime = System.currentTimeMillis()
      var i = 0
      try {
        while (i < 10) {
          yield()
          if (System.currentTimeMillis() >= nexTime) {
            println("i = $i isActive = $isActive")
            i++
            nexTime += 500
          }
        }
      } catch (e: CancellationException) {
        println("補捉到CancellationException Exception")
      }
    }
    delay(100)
    job1.cancel()
    job1.join()
  }
{% endhighlight %}
```
i = 0 isActive = true
補捉到CancellationException Exception
```
## 超時處理
### withTimeout(ms)
以下程式碼執行超過3秒就會被cancel()取消。<br>
會產生TimeoutCancellationException。<br>
{% highlight kotlin linenos %}
fun coroutin25() = runBlocking {
  withTimeout(3000) {
    repeat(10) { i ->
      println("i = $i")
      delay(1000)
    }
  }
}
{% endhighlight %}
```
i = 0
i = 1
i = 2

Timed out waiting for 3000 ms
kotlinx.coroutines.TimeoutCancellationException: Timed out waiting for 3000 ms
```

### withTimeoutOrNull
超時就傳回null，沒有超時就傳回finish
{% highlight kotlin linenos %}
@Test
fun coroutin25() = runBlocking {
  val timeout = withTimeoutOrNull(3000) {
    repeat(10) { i ->
      println("i = $i")
      delay(1000)
    }
    // 完成的文字
    "finish"
  }
  println("result = $timeout")
}
{% endhighlight %}
```
i = 0
i = 1
i = 2
result = null
```

