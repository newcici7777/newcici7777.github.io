---
title: 協程作用域
date: 2025-09-22
keywords: kotlin, coroutine scope
---
## Scope獨立作用域
### Scope獨立作用域與沒有獨立作用域

|特性 |沒有 Scope (launch{}) |有 Scope (scope.launch{})|
|父子關係| 與 runBlocking 是父子|與 runBlocking 無父子關係|
|join| 自動join()|手動join()|
|調度器|繼承父協程|使用自定義調度器|

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
}
{% endhighlight %}

### CoroutineScope
建立一個獨立CoroutineScope作用域，runBlocking的作用域為TestScope。<br>

CoroutineScope下面有一個子協程，名字為job1。<br>

![img]({{site.imgurl}}/kotlin/scope_extend1.png)<br>

調度器設為預設Dispatchers.Default。<br>

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
runBlocking協程與作用域協程，沒有父子關係，所以runBlocking協程執行完畢就結束，<span class="markline">不會等待</span>CoroutineScope下的job1子協程執行完。<br>

#### delay暫停
增加一個delay(大於1000)，因為CoroutineScope下的子協程執行時為1秒，要讓TestScope(runBlocking)暫停超過1秒，再結束runBlocking協程。<br>

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

#### join 等待
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

### GlobalScope
GlobalScope也是獨立作用域，加上join，runBlocking才會等待GlobalScope執行完畢。
{% highlight kotlin linenos %}
  @Test
  fun coroutin08() = runBlocking {
    val job = GlobalScope.launch {
      delay(1000)
      println("job1")
    }
    job.join()
  }
{% endhighlight %}
```
job1
```

### Scope使用時機
{% highlight kotlin linenos %}
// 場景 1: 需要自定義調度器
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

## coroutineScope runBlocking
coroutineScope雖然名字有Scope，但開頭字母為小寫，不是獨立作用域，父親是runBlocking，runBlocking是TestScope的作用域。<br>

![img]({{site.imgurl}}/kotlin/scope_extend2.png)<br>

### 2個suspend函式:
{% highlight kotlin linenos %}
  private suspend fun doOne():Int {
    // 1秒
    delay(1000)
    return 10
  }
  private suspend fun doTwo():Int {
    // 2秒
    delay(1000)
    return 20
  }
{% endhighlight %}

### coroutineScope 非阻塞協程
以下程式碼，二個suspend函式同時執行。<br>
coroutineScope會等待子協程(suspend 函式)，執行完畢。<br>
{% highlight kotlin linenos %}
  fun coroutin05() = runBlocking {
    val startTime = System.currentTimeMillis()
    coroutineScope {
        val one = doOne()
        val two = doTwo()
    }
    val duration = System.currentTimeMillis() - startTime
    println(" in ${duration} ms")
  }
{% endhighlight %}
```
 in 2 ms
```
### runBlocking 阻塞協程
以下程式碼，二個suspend函式逐一執行，doOne()執行完，才輪到doTwo執行()。<br>
所以共執行約2秒鐘左右。<br>
runBlocking會等待子協程(suspend 函式)，執行完畢。<br>
{% highlight kotlin linenos %}
  fun coroutin05() = runBlocking {
    val startTime = System.currentTimeMillis()
    runBlocking {
        val one = doOne()
        val two = doTwo()
    }
    val duration = System.currentTimeMillis() - startTime
    println(" in ${duration} ms")
  }
{% endhighlight %}
```
 in 2009 ms
```
