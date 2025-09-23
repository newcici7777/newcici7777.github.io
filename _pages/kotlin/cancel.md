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

join變成<span class="markline">等待取消</span>。<br>
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

## Cpu運算無法被取消




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
