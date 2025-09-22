---
title: 協程
date: 2025-09-19
keywords: kotlin, launch, async
---
## launch async
### 主協程 父協程
等號右邊=runBlocking，代表runBlocking是主協程。
{% highlight kotlin linenos %}
  fun coroutin01() = runBlocking {}
{% endhighlight %}

### 子協程
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
{% endhighlight %}

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
  fun coroutin04() = runTest {
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
  fun coroutin03() = runTest {
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

## Job
launch()與async()，傳回值是Job物件，Job物件管理協程的生命周期。<br>

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

### Job生命周期
Job的狀態有New(建立)、Active、Completing(正要完成中)、Completed(已完成)、Cancelling(取消中)、Cancelled(已取消)。<br>

job的對映屬性是:isActive、isCancelled、isCompleted。<br>

## Scope協程
以下自建Scope協程，調度器設為預設Dispatchers.Default。<br>
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
runBlocking協程與Scope協程，沒有父子關係，所以runBlocking協程執行完畢就結束，<span class="markline">不會等待</span>scope.launch{}協程執行完，才結束。<br>

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
GlobalScope也是自建的Scope，必須加上join，runBlocking才會等待GlobalScope執行完畢。
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

## join 與 cancel
以下的程式碼cancel()不會立刻馬上取消，而join會轉變成「等待」協程「cancel()取消完成」，確保job協程「執行完畢」。<br>
{% highlight kotlin linenos %}
  fun coroutin08() = runBlocking {
    val job = GlobalScope.launch {
        delay(1000)
        println("job1")
    }
    // 暫停0.1秒
    delay(100)
    // 取消協程
    job.cancel()
    // 此時變成主協程等待job完成取消流程
    job.join()
  }
{% endhighlight %}

如果只有cancel，協程正在清理資料，但主協程執行完了，就退出了。
```
job.cancel()
```

需要二者一起搭配。
{% highlight kotlin linenos %}
    // 取消協程
    job.cancel()
    // 此時變成主協程等待job完成取消流程
    job.join()
{% endhighlight %}

也可以使用cancelAndJoin()取代。
{% highlight kotlin linenos %}
job.cancelAndJoin()
{% endhighlight %}

### delay()取消流程
因為 delay() 是一個可取消的掛起函數，當協程被取消時：

1. delay() 會拋出 CancellationException
2. 協程進入異常處理流程 (catch 區塊)
3. 協程正常結束

### exception
取消協程不會「顯示」任何exception，但實際上會拋出CancellationException。
{% highlight kotlin linenos %}
  fun coroutin08() = runBlocking {
    val job = GlobalScope.launch {
        delay(1000)
        println("job1")
    }
    // 暫停0.1秒
    delay(100)
    // 取消協程
    job.cancel()
    // 此時變成「等待」等待它完成取消流程
    job.join()
  }
{% endhighlight %}

加上try{}... catch{}就可以抓出CancellationException。<br>
而join已經變成「等待
{% highlight kotlin linenos %}
  fun coroutin08() = runBlocking {
    val job = GlobalScope.launch {
      try {
        delay(1000)
        println("job1")
      }catch (e:Exception) {
        e.printStackTrace()
      }
    }
    delay(100)
    job.cancel(CancellationException("自訂取消Exception"))
    job.join()
  }
{% endhighlight %}
```
java.util.concurrent.CancellationException: 自訂取消Exception
  at com.example.coroutine.Test01$coroutin08$1.invokeSuspend(Test01.kt:105)
```
