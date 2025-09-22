---
title: 協程作用域
date: 2025-09-22
keywords: kotlin, coroutine scope
---
## coroutineScope 與 runBlocking 
2個suspend函式:
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
  fun coroutin05() = runTest {
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
  fun coroutin05() = runTest {
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

## 子協程失敗
- coroutinScope 子協程失敗，其它子協程會取消
- supervisorScope 子協程失敗，不會影嚮其它子協程

### coroutineScope
job2 拋出Exception()，導致job1被cancel()，不會執行job1 finish。
{% highlight kotlin linenos %}
  fun coroutin06() = runTest {
    coroutineScope {
      val job1 = launch {
        delay(400)
        println("job1 finish")
      }
      val job2 = async{
        delay(200)
        println("job2 finish")
        throw IllegalArgumentException()
      }
    }
  }
{% endhighlight %}
```
job2 finish

java.lang.IllegalArgumentException
  at com.example.coroutine.Test01$coroutin06$1$1$job2$1.invokeSuspen
```
### supervisorScope
job2 拋出Exception()，job1仍會執行完畢。
{% highlight kotlin linenos %}
  fun coroutin06() = runTest {
    supervisorScope {
      val job1 = launch {
        delay(400)
        println("job1 finish")
      }
      val job2 = async{
        delay(200)
        println("job2 finish")
        throw IllegalArgumentException()
      }
    }
  }
{% endhighlight %}
```
job2 finish
job1 finish
```

## Scope作用域
以下自建Scope作用域，調度器設為預設Dispatchers.Default。<br>
使用<span class="markline">scope.</span>launch{}，使用的是自建Scope協程。<br>

{% highlight kotlin linenos %}
  fun coroutin07() = runBlocking {
    val scope = CoroutineScope(Dispatchers.Default)
    scope.launch {
      delay(1000)
      println("job1")
    }
  }
{% endhighlight %}

但執行完卻沒有任何結果，這是為什麼？<br>
runBlocking協程與作用域協程，沒有父子關係，所以runBlocking協程執行完畢就結束，<span class="markline">不會等待</span>scope.launch{}協程執行完，才結束。<br>

### delay
增加一個delay(大於1000)
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

### join
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
GlobalScope也是作用域，加上join，runBlocking才會等待GlobalScope執行完畢。
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
