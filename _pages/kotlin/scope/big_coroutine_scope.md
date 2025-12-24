---
title: Coroutin Scope 手動管理
date: 2025-10-14
keywords: Android, Kotlin, coroutines, Coroutin Scope
---
Prerequisites:

- [Dispatchers][1]

使用Coroutin Scope時機:
- 自訂Dispatchers
- 需要自訂錯誤處理
- 需要手動取消Scope下面的子協程

## 建立CoroutineScope
建立一個獨立CoroutineScope，並且自訂Dispatchers。

runBlocking的Scope作用域是BlockingCoroutine。

CoroutineScope下面有一個子協程，名字為job1。<br>

![img]({{site.imgurl}}/kotlin/scope_extend1.png)<br>

Dispatchers設為預設Dispatchers.Default。<br>

使用<span class="markline">scope.</span>launch{}，建立子協程並啟動子協程。<br>
{% highlight kotlin linenos %}
@Test
fun coroutin07() = runBlocking {
  val scope = CoroutineScope(Dispatchers.Default)
  val job1 = scope.launch {
    delay(1000)
    println("job1")
  }
}
{% endhighlight %}

但執行完卻沒有任何結果，這是為什麼？<br>
runBlocking協程與CoroutineScope，沒有父子關係，所以runBlocking協程執行完畢就結束，<span class="markline">不會等待</span>CoroutineScope下的job1子協程執行完。<br>

### delay暫停
增加一個delay(大於1000)，因為CoroutineScope下的子協程執行時為1秒，要讓runBlocking暫停超過1秒，再結束runBlocking協程。<br>

delay()參數要大於1000，等到scope執行完畢，runBlocking協程才能結束。<br>

{% highlight kotlin linenos %}
  fun coroutin07() = runBlocking {
    val scope = CoroutineScope(Dispatchers.Default)
    scope.launch {
      delay(1000)
      println("job1")
    }
    delay(2000)
  }
{% endhighlight %}
```
job1
```

### join 等待
runBlocking協程「等待」scope.job完成，runBlocking才能結束。
{% highlight kotlin linenos %}
fun coroutin07() = runBlocking {
  val scope = CoroutineScope(Dispatchers.Default)
  val job = scope.launch {
    delay(1000)
    println("job1")
  }
  job.join()
}
{% endhighlight %}
```
job1
```

## 有CoroutineScope 與 沒有CoroutineScope

|特性 |沒有 CoroutineScope launch{} |有 CoroutineScope scope.launch{}|
|父子關係| 與 runBlocking 是父子|與 runBlocking 無父子關係|
|join| 自動join()|手動join()|
|Dispatchers|繼承runBlocking預設的Dispatchers|使用自訂Dispatchers|

join
{% highlight kotlin linenos %}
// 沒有 Scope - 自動join
fun example1() = runBlocking {
    launch {
        delay(1000)
        println("一定會執行") // runBlocking 會等待
    }
    // 自動等待所有子協程
}

// 有 Scope - 自己寫join()  
fun example2() = runBlocking {
    val scope = CoroutineScope(Dispatchers.Default)
    scope.launch {
        delay(1000)
        println("可能不會執行！") // 如果 runBlocking 先結束
    }
    // runBlocking 結束時不會等待 scope 中的協程
    // 需自己寫join()，runBlocking才會等待。
}
{% endhighlight %}

### CoroutineScope使用時機
{% highlight kotlin linenos %}
// 場景 1: 需要自訂Dispatchers
fun processInBackground() {
    val ioScope = CoroutineScope(Dispatchers.IO)
    ioScope.launch {
        // 在IO線程執行網路請求
    }
}

// 場景 2: 長時間運行的後台任務
class MyService {
    private val serviceScope = CoroutineScope(Dispatchers.Default)
    
    fun start() {
        serviceScope.launch {
            while (isActive) {
                // 長時間運行的任務
                delay(5000)
            }
        }
    }
    
    fun stop() {
        serviceScope.cancel() // 需要手動取消
    }
}

// 場景 3: 在非協程環境中啟動協程
fun nonCoroutineFunction() {
    val scope = CoroutineScope(Dispatchers.Main)
    scope.launch {
        // 在Android主線程更新UI
    }
}
{% endhighlight %}

[1]: {% link _pages/kotlin/context.md %}