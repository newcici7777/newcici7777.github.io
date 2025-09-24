---
title: cancel
date: 2025-09-22
keywords: kotlin, Coroutine cancel
---
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
  fun coroutin08() = runTest {
    val job = Job()
    val child = launch(job) {
        delay(1000)
        println("child finish")
    }
    // 暫停0.1秒
    delay(100)
    // 取消協程
    child.cancel()
    // runTest等待完成取消
    child.join()
  }
{% endhighlight %}

如果只有cancel，協程正在清理資料，但runTest執行完了，就退出了。
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
  fun coroutin08() = runTest {
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
    // runTest等待完成取消
    child.join()
  }
{% endhighlight %}
```
kotlinx.coroutines.JobCancellationException: StandaloneCoroutine was cancelled; job="coroutine#3":StandaloneCoroutine{Cancelling}@ff684e1
```

### isActive判斷子協程是否被取消
isActive會傳回協程是否正在運行中。<br>
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

## 作用域Scope 取消
下面的程式碼是，GlobalScope獨立作用域的取消。<br>
取消協程不會「顯示」任何exception，但實際上會拋出CancellationException。<br>

### 取消作用域Scope
作用域scope.cancel()，會直接把相同作用域的協程取消。<br>
{% highlight kotlin linenos %}
@Test
fun coroutin19() = runTest {
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
fun coroutin19() = runTest {
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
  fun coroutin08() = runTest {
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
finally是不管如何都會執行，可在finally中釋放資源。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin22() = runTest {
    var br: BufferedReader? = null
    try {
      br = BufferedReader(FileReader("/Users/cici/testc/file_test"))
    } finally {
      br?.close()
    }
  }
{% endhighlight %}

### withContext(NonCancellable)
被cancel的協程中，在finally有suspend函式，不會執行。<br>
以下child1被取消，不會印出「finally 2」。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin24() = runTest {
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

改用withContext(NonCancellable)包住suspend函式就可以。
{% highlight kotlin linenos %}
  @Test
  fun coroutin24() = runTest {
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
fun coroutin23() = runTest {
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
  fun coroutin21() = runTest {
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

### isActive
加上isActive判斷Job是否可以執行。<br>

執行結果只印出i = 0，不會一直印出。<br>

加上try ... catch ... 補捉CancellationException的例外。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin21() = runTest {
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
Job不是Active，ensureActive()會拋出JobCancellationException，協程有例外就會停止。<br>

加上try ... catch ... 補捉CancellationException的例外。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin21() = runTest {
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
Job不是Active，yield()會拋出JobCancellationException，協程有例外就會停止，並讓出cpu使用權。
{% highlight kotlin linenos %}
  fun coroutin21() = runTest {
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

## Scope獨立作用域與沒有獨立作用域

|特性 |沒有 Scope (launch{}) |有 Scope (scope.launch{})|
|父子關係| 與 runTest 是父子|與 runTest 無父子關係|
|join| 自動join()|手動join()|
|取消傳播| 自動傳播 |不會自動傳播|
|異常處理|自動傳播異常|獨立異常處理|
|調度器|繼承父協程|使用自定義調度器|

join
{% highlight kotlin linenos %}
// 沒有 Scope - 自動join
fun example1() = runTest {
    launch {
        delay(1000)
        println("一定會執行") // runTest 會等待
    }
    // 自動等待所有子協程
}

// 有 Scope - 自己寫join()  
fun example2() = runTest {
    val scope = CoroutineScope(Dispatchers.Default)
    scope.launch {
        delay(1000)
        println("可能不會執行！") // 如果 runTest 先結束
    }
    // runTest 結束時不會等待 scope 中的協程
}
{% endhighlight %}
