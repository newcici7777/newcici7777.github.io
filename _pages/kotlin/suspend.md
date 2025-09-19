---
title: suspend
date: 2025-09-19
keywords: kotlin, suspend
---
## runBlocking launch async
runBlocking是主協程，裡面包含了job1與job2二個子協程。<br>
runBlocking會等待job1與job2執行完畢。<br>
launch、async都是同時執行，job2不用等待job1執行完畢才執行。<br>
{% highlight kotlin linenos %}
  fun coroutin01() = runBlocking {
    val job1 = launch {
      delay(200)
      println("job1 finished")
    }
    val job2 = async {
      delay(200)
      println("job2 finished")
    }
  }
{% endhighlight %}
```
job1 finished
job2 finished
```

### await() 傳回值
語法
{% highlight kotlin linenos %}
job2.await()
{% endhighlight %}

async會傳回最後一行執行結果，並讓主協程等待job2子協程執行完畢。
{% highlight kotlin linenos %}
  fun coroutin01() = runBlocking {
    val job1 = launch {
      delay(200)
      println("job1 finished")
    }
    val job2 = async {
      delay(200)
      println("job2 finished")
      // 回傳結果
      "job2 result"
    }
    // 印出回傳結果
    println(job2.await())
  }
{% endhighlight %}
```
job1 finished
job2 finished
job2 result
```

### await()執行完才輪其它協程
```
job1.await()
```
job1先執行，job1執行完，才輪job2與job3，job2、job3同時執行。(程式碼移動到那一行，就執行)
{% highlight kotlin linenos %}
  fun coroutin01() = runBlocking {
    val job1 = launch {
      delay(200)
      println("job1 finished")
    }
    // job1 先執行
    job1.await()
    val job2 = async {
      delay(200)
      println("job2 finished")
    }
    val job3 = async {
      delay(200)
      println("job3 finished")
    }
  }
{% endhighlight %}

### join()執行完才輪其它協程
```
job1.join()
```
job1先執行，job1執行完，才輪job2與job3，job2、job3同時執行。(程式碼移動到那一行，就執行)
{% highlight kotlin linenos %}
  fun coroutin01() = runBlocking {
    val job1 = launch {
      delay(200)
      println("job1 finished")
    }
    job1.join()
    val job2 = async {
      delay(200)
      println("job2 finished")
    }
    val job3 = async {
      delay(200)
      println("job3 finished")
    }
  }
{% endhighlight %}

### 逐步執行與同時執行
#### 沒加async是逐步執行
measureTimeMillis()是計算執行時間。<br>
doOne()、doTwo()都是suspend 協程函式。<br>
但在這邊會doOne()執行完，才輪到doTwo()執行。<br>
所以耗時2秒多。<br>
{% highlight kotlin linenos %}
  fun coroutin02() = runBlocking {
    val time = measureTimeMillis {
      val one = doOne()
      val two = doTwo()
      println("result = ${one + two}")
    }
    println("total in $time ms")
  }
  private suspend fun doOne():Int {
    // 停1秒
    delay(1000)
    return 10
  }
  private suspend fun doTwo():Int {
    // 停1秒
    delay(1000)
    return 20
  }
{% endhighlight %}
```
result = 30
total in 2022 ms
```
#### 加async是同時執行
{% highlight kotlin linenos %}
  fun coroutin02() = runBlocking {
    val time = measureTimeMillis {
      val one = async{doOne()}
      val two = async{doTwo()}
      println("result = ${one.await() + two.await()}")
    }
    println("total in $time ms")
  }
  private suspend fun doOne():Int {
    delay(1000)
    return 10
  }
  private suspend fun doTwo():Int {
    delay(1000)
    return 20
  }
{% endhighlight %}
```
result = 30
total in 1020 ms
```

#### await()傳回值錯誤範例
await()放在async{}後面，會導致doOne()執行完畢，doTwo()才能執行，變成逐步執行，而不是同時執行。
{% highlight kotlin linenos %}
  fun coroutin02() = runBlocking {
    val time = measureTimeMillis {
      val one = async{doOne()}.await()
      val two = async{doTwo()}.await()
      println("result = ${one + two}")
    }
    println("total in $time ms")
  }
  private suspend fun doOne():Int {
    delay(1000)
    return 10
  }
  private suspend fun doTwo():Int {
    delay(1000)
    return 20
  }
{% endhighlight %}
```
result = 30
total in 2024 ms
```
### launch async父類別
launch繼承Job
{% highlight kotlin linenos %}
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job 
{% endhighlight %}

async繼承Deferred，Deferred繼承Job，所以async父類別也是job。
{% highlight kotlin linenos %}
public fun <T> CoroutineScope.async(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> T
): Deferred<T> 
{% endhighlight %}

{% highlight kotlin linenos %}
public interface Deferred<out T> : Job
{% endhighlight %}

## 啟動模式的設定
所謂的啟動模式，也就是下面的start參數，async()與launch()都有start<br>
{% highlight kotlin linenos %}
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job 
{% endhighlight %}

