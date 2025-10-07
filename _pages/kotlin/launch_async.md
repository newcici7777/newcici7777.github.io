---
title: launch async
date: 2025-09-19
keywords: kotlin, launch, async
---
## launch與async 傳回Job
launch()與async()是建立子協程，並且啟動執行子協程。<br>
launch()與async()，傳回值是Job物件，Job物件管理協程的生命周期。<br>

### launch async同時執行
runBlocking是父協程，裡面包含了job1與job2二個子協程。<br>
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

### launch
傳回值是Job。
{% highlight kotlin linenos %}
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job 
{% endhighlight %}

### async
async傳回值是Deferred。
{% highlight kotlin linenos %}
public fun <T> CoroutineScope.async(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> T
): Deferred<T> 
{% endhighlight %}

Deferred繼承Job，所以Deferred也是job。
{% highlight kotlin linenos %}
public interface Deferred<out T> : Job
{% endhighlight %}

#### await() 傳回值
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

#### await()執行完才輪其它協程
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
```
job1 finished
job2 finished
job3 finished
```
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
```
job1 finished
job2 finished
job3 finished
```
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

## 協程取消與啟動模式
啟動模式，也就是下面的start參數，async()與launch()都有start<br>
{% highlight kotlin linenos %}
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job 
{% endhighlight %}

### start = DEFAULT
job 10秒後才會執行，但在執行前就被主協程取消，所以10秒後不會印出job finish。
{% highlight kotlin linenos %}
  fun coroutin03() = runBlocking {
    val job = launch(start = CoroutineStart.DEFAULT) {
      println("test")
      delay(10000)
      println("job finish")
    }
    job.cancel()
    println("job cancel")
  }
{% endhighlight %}
```
job cancel
```

### start = ATOMIC
delay()是suspend函式，若start設為ATOMIC，即便主協程把子協程取消，子協程會執行到第1個suspend函式，才會執行取消，在suspend函式之前的程式碼都會執行。<br>

下面程式執行結果仍會印出test，因為子協程job是到delay()才被取消，delay()之前的程式碼都會執行。
{% highlight kotlin linenos %}
  fun coroutin03() = runBlocking {
    val job = launch(start = CoroutineStart.ATOMIC) {
      println("test")
      delay(10000)
      println("job finish")
    }
    job.cancel()
    println("job cancel")
  }
{% endhighlight %}
```
job cancel
test
```

### start = Lazy
start設為Lazy，遇到start()、await()、join()才會建立協程，用到的時候才會建立函式不占記憶體function stack空間。<br>

但因為我的子協程內容過短，會讓編譯器認為沒內容，編譯會失敗，使用runTest取代runBlocking當主協程。<br>

build.gradle要加上以下內容。
{% highlight kotlin linenos %}
testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test:1.7.3")
{% endhighlight %}

要import runTest
{% highlight kotlin linenos %}
import kotlinx.coroutines.test.runTest
{% endhig```
hlight %}

主要程式內容
{% highlight kotlin linenos %}
  @Test
  fun coroutin03() = runTest {
    val job = async(start = CoroutineStart.LAZY) {
      println("job finish")
    }
    // 執行到start() await() join()，才建立job協程
    job.start()
    // job.await()
  }
{% endhighlight %}
```
job finish
```

### UNDISPATCHED執行緒切換
#### start = DEFAULT
以下start為DEFAULT預設，context協程調度器設為IO，IO為在背景處理耗時操作。<br>

job協程建立時，從Thread Pool中取出IO執行緒，在此運行。<br>

1. coroutin04()一開始執行緒為Test worker。
2. 執行job協程，執行緒是在IO執行緒(DefaultDispatcher-worker-1)。
2. 呼叫suspend函式後，執行緒在IO執行緒(DefaultDispatcher-worker-1)。
{% highlight kotlin linenos %}
  fun coroutin04() = runBlocking {
    println("目前thread = " + Thread.currentThread().name)
    val job = async(context = Dispatchers.IO, start = CoroutineStart.DEFAULT) {
      println("協程 thread = " + Thread.currentThread().name)
      // 呼叫suspend函式
      delay(1000)
      println("suspend resume thread = " + Thread.currentThread().name)
    }
  }
{% endhighlight %}
```
目前thread = Test worker @kotlinx.coroutines.test runner#2
協程 thread = DefaultDispatcher-worker-1 @coroutine#3
suspend resume thread = DefaultDispatcher-worker-1 @coroutine#3
```

#### start = UNDISPATCHED 
以下start為UNDISPATCHED，context協程調度器設為IO，IO為在背景處理耗時操作。<br>

job協程建立時，與coroutin03()的執行緒相同都是Test worker。<br>
呼叫suspend函式後，才會轉成IO執行緒(DefaultDispatcher-worker-1)執行。<br>

1. coroutin03()的主執行緒的名字為Test worker。
2. job協程仍在主執行緒(Test work)執行。
2. 呼叫suspend函式後，執行緒切換至IO執行緒(DefaultDispatcher-worker-1)。
{% highlight kotlin linenos %}
  fun coroutin03() = runBlocking {
    println("目前thread = " + Thread.currentThread().name)
    val job = async(context = Dispatchers.IO, start = CoroutineStart.UNDISPATCHED) {
      println("協程 thread = " + Thread.currentThread().name)
      // 呼叫suspend函式
      delay(1000)
      println("suspend resume thread = " + Thread.currentThread().name)
    }
  }
{% endhighlight %}
```
目前thread = Test worker @kotlinx.coroutines.test runner#2
協程 thread = Test worker @coroutine#3
suspend resume thread = DefaultDispatcher-worker-1 @coroutine#
```

### 執行
DEFAULT、ATOMIC、LAZY，執行到子協程不會立刻執行，只會先開始建立。<br>
UNDISPATCHED會立刻建立協程，並立刻執行。<br>





