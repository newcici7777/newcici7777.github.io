---
title: Job
date: 2025-09-19
keywords: kotlin, job
---
中文為工作、任務。<br>

## import
{% highlight groovy linenos %}
import kotlinx.coroutines.CoroutineDispatcher
import kotlinx.coroutines.CoroutineName
import kotlinx.coroutines.Job
import kotlinx.coroutines.job
import kotlinx.coroutines.runBlocking
import org.junit.Test
{% endhighlight %}

## Job 生命周期
Job的狀態有New(建立)、Active、Completing(正要完成中)、Completed(已完成)、Cancelling(取消中)、Cancelled(已取消)。<br>

### Job生命周期圖
1. New 建立: Job建立。
2. Active 運行中: Job.start()啟動。
3. child子協程加入job並啟動。
4. Completing 完成中: child.join()等待子協程完成。
5. Completed 已完成: Job與child子協程都執行完畢。

![img]({{site.imgurl}}/kotlin/job_lifecycle.png)<br>

生命周期屬性是:isActive(是否執行中)、isCancelled(是否取消)、isCompleted(是否完成)。<br>

### 建立空的Job
{% highlight kotlin linenos %}
val job = Job()
{% endhighlight %}

### 啟動Job
{% highlight kotlin linenos %}
@Test
fun coroutin10() = runBlocking {
  // New
  val job = Job()
  // Start
  job.start()
  println("job = ${job.isActive}")
}
{% endhighlight %}
```
job = true
```
### 建立啟動子協程
launch()是建立並啟動新協程，launch()參數是父親job，代表這個新協程是job的子協程。<br>

協程是通過launch協程構建器創建的，它運行指定的代碼塊，並在該代碼塊完成時結束。<br>
```
launch(job) { 要執行的程式 }
```

launch會自動啟動start()。
{% highlight kotlin linenos %}
val child = launch(job) {
  delay(1000)
}
{% endhighlight %}

launch程式碼，會自動呼叫start()
{% highlight kotlin linenos %}
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job {
    val newContext = newCoroutineContext(context)
    val coroutine = if (start.isLazy)
        LazyStandaloneCoroutine(newContext, block) else
        StandaloneCoroutine(newContext, active = true)
    // launch會呼叫start()
    coroutine.start(start, coroutine, block)
    return coroutine
}
{% endhighlight %}

### runBlocking 等待協程執行完畢
join()代表等待，runBlocking{}是一個協程，裡面有一個協程child在執行，注意child的爸爸是Job，不是runBlocking，runBlocking與Job二者是不相同，runBlocking與child沒有父子關係，純粹就是<span class="markline">等待協程執行完畢</span>。<br>

![img]({{site.imgurl}}/kotlin/job_extend1.png)<br>

```
fun coroutin10() = runBlocking {
  .
  .
  .
  // 等待協程執行完畢
  child.join()
  // 子協程已執行完畢
  .
  .
  .
}
```

### 子協程完成狀態
在join()之前，子協程仍在執行中，join()之後，子協程執行完畢。<br>
但即便子協程執行完畢，Job是父親，isActive = true 狀態仍在執行中，並未完成。<br>
{% highlight kotlin linenos %}
@Test
fun coroutin10() = runBlocking {
  // New 建立
  val job = Job()
  // Start 啟動
  job.start()
  println("job isActive= ${job.isActive}")
  // job加入子協程，並啟動子協程
  val child = launch(job) {
    delay(1000)
  }
  // 子協程執行中
  println("child isActive= ${child.isActive}")
  // 等待子協程完成
  // wait children complete
  child.join()
  // 子協程已完成
  // children completed
  println("child isActive= ${child.isActive}")
  println("child isCompleted= ${child.isCompleted}")
  // job仍未完成，仍在完成中...
  // job Completing
  println("job isActive= ${job.isActive}")
}
{% endhighlight %}
```
job isActive= true
child isActive= true
child isActive= false
child isCompleted= true
job isActive= true
```

### 手動完成
job.complete()可以把Job狀態變成已完成。
{% highlight kotlin linenos %}
if (job is CompletableJob) {
  job.complete()
  println("After complete job isActive = ${job.isActive}")
  println("After complete job isCompleted= ${job.isCompleted}")
}
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
@Test
fun coroutin10() = runBlocking {
  // New
  val job = Job()
  // Start
  job.start()
  println("job isActive= ${job.isActive}")
  val child = launch(job) {
    delay(1000)
  }
  println("child isActive= ${child.isActive}")
  // wait children complete
  child.join()
  // children completed
  println("child isActive= ${child.isActive}")
  println("child isCompleted= ${child.isCompleted}")
  // job Completing
  println("job isActive= ${job.isActive}")
  if (job is CompletableJob) {
    job.complete()
    println("After complete job isActive = ${job.isActive}")
    println("After complete job isCompleted= ${job.isCompleted}")
  }
}
{% endhighlight %}
```
job isActive= true
child isActive= true
child isActive= false
child isCompleted= true
job isActive= true
After complete job isActive = false
After complete job isCompleted= true
```

## Job父子關係
### coroutineContext
以下二種方法都是取得目前的Job物件，Job物件為launch{}的傳回值。

{% highlight kotlin linenos %}
coroutineContext.job
coroutineContext[Job]
{% endhighlight %}

若試圖輸出coroutineContext.job，會有以下格式。<br>
```
Job名字 #編號: 協程作用域 {Job狀態} @ Job物件hashCode
coroutine#1":BlockingCoroutine{Active}@7674b62c
```

|每一個區塊|  含義|
|coroutine#1| 協程的內部編號（第一個被建立的 coroutine）|
|BlockingCoroutine| 協程的類型（這裡是由 runBlocking 建立的 coroutine）|
|{Active}|  協程目前的狀態：可能是 Active、Completed、Cancelled 等|
|@7674b62c| 物件的 hash code（十六進位表示）|

下圖中，綠色的Job物件為runBlocking{}的傳回值為BlockingCoroutine。<br>
紅色的Job物件為launch{}的傳回值child。<br>
![img]({{site.imgurl}}/kotlin/get_job.png)<br>

執行結果可以知道job的父親是null，child的父親是job。<br>

![img]({{site.imgurl}}/kotlin/job_extend1.png)<br>

而runBlocking傳回的是BlockingCoroutine。<br>
child的祖父不是BlockingCoroutine，都是分開來。<br>
{% highlight kotlin linenos %}
  fun coroutin11() = runBlocking {
    println("runBlocking = ${coroutineContext[Job]}")
    println("runBlocking = ${coroutineContext.job}")
    val job = Job()
    job.start()
    println("job parent = ${job.parent}")
    println("job = $job")

    val child = launch(job) {
      delay(1000)
      println("inner child = ${coroutineContext[Job]}")
      println("inner child = ${coroutineContext.job}")
    }
    println("child parent= ${child.parent}")
    println("child = ${child}")
    child.join()
  }
{% endhighlight %}
```
runBlocking = "coroutine#1":BlockingCoroutine{Active}@7674b62c
runBlocking = "coroutine#1":BlockingCoroutine{Active}@7674b62c
job parent = null
job = JobImpl{Active}@5e4bd84a
child parent= JobImpl{Active}@5e4bd84a
child = "coroutine#3":StandaloneCoroutine{Active}@2a62b5bc
inner child = "coroutine#3":StandaloneCoroutine{Active}@2a62b5bc
inner child = "coroutine#3":StandaloneCoroutine{Active}@2a62b5bc
```

#### isActive isCompleted isCancelled
coroutineContext.job有isActive、isCompleted、isCancelled屬性。
{% highlight kotlin linenos %}
    println("isActive: ${coroutineContext.job.isActive}")
    println("isCompleted: ${coroutineContext.job.isCompleted}")
    println("isCancelled: ${coroutineContext.job.isCancelled}")
{% endhighlight %}

### Job中的協程數量
取得Job中尚未完成子協程數量。<br>
```
Job名.children.count()
```

childJob的父親是parent，childJob下面有2個子協程，分別是child1與child2。<br>
child2會先執行完畢，因為它才delay2秒，此時的childJob.children.count() = 1，代表剩下一個子協程未完成。<br>

等到delay 3秒後，child1與child2全執行完畢，childJob的狀態也變成已完成。`isCompleted = true`<br>
{% highlight kotlin linenos %}
fun coroutin09() = runBlocking {
  val parent = Job()
  val childJob = launch(parent) {
    val child1 = launch {
      delay(3000)
      println("child1 finish")
    }
    val child2 = async {
      delay(2000)
      println("child2 finish")
    }
    child1.start()
    child2.start()
  }
  // 2.1秒後
  delay(2100)
  println("childJob isActive = ${childJob.isActive}")
  println("childJob 尚未完成數量 = ${childJob.children.count()}")
  // 2.1秒+1秒 = 3.1秒後
  delay(1000)
  println("childJob 尚未完成數量 = ${childJob.children.count()}")
  println("childJob isActive = ${childJob.isActive}")
  println("childJob isCompleted = ${childJob.isCompleted}")
}
{% endhighlight %}
```
child2 finish
childJob isActive = true
childJob 尚未完成數量 = 1
child1 finish
childJob 尚未完成數量 = 0
childJob isActive = false
childJob isCompleted = true
```

## job啟動所有子協程
runBlocking對listof的物件，不會自動start()。<br>
本例使用runBlocking。<br>

job是所有list中的子協程的父親，job.start()，所有子協程全啟動。<br>
使用it.join()讓runBlocking協程等待所有協程執行完畢。<br>
注意！runBlocking不是list中子協程的父親，runBlocking只負責「等待」別的協程執行完畢。<br>
{% highlight kotlin linenos %}
  fun coroutin13() = runBlocking {
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
    list.forEach { it.join() }
    println("all children finish.")
  }
{% endhighlight %}
```
list[0] finish
list[1] finish
all children finish.
```

## 未指派父親Job
等號右邊 = runBlocking，launch{}沒給任何job父親參數，預設runBlocking為job1的父協程。<br>

![img]({{site.imgurl}}/kotlin/job_extend2.png)<br>

因為runBlocking是父協程，父協程會自動<span class="markline">等待</span>子協程job1執行完畢，不用加上job1.join()，因為二者是父子關係。
{% highlight kotlin linenos %}
fun coroutin01() = runBlocking {
  val job1 = launch {
    delay(200)
    println("job1 finished")
  }
}
{% endhighlight %}
```
job1 finished
```

## this
如果只用this會混淆這個this物件是誰，可以使用`@`標籤名，來區別這個this是誰。<br>

可以使用以下方法，印出物件的類別。
```
this@launch::class.simpleName
```
{% highlight kotlin linenos %}
@Test
fun coroutin12() = runBlocking {
  println("outside this = $this")
  val job = Job()
  val child = launch(job) {
    println("inner this = $this")
    println("launch this = ${this@launch}")
    println("runBlocking this = ${this@runBlocking}")
    println("launch class = ${this@launch::class.simpleName}")
    println("runBlocking class = ${this@runBlocking::class.simpleName}")
  }
  child.join()
}
{% endhighlight %}
```
outside this = "coroutine#1":BlockingCoroutine{Active}@662706a7
inner this = "coroutine#2":StandaloneCoroutine{Active}@488d1cd7
launch this = "coroutine#2":StandaloneCoroutine{Active}@488d1cd7
runBlocking this = "coroutine#1":BlockingCoroutine{Active}@662706a7
launch class = StandaloneCoroutine
runBlocking class = BlockingCoroutine
```

[1]: {% link _pages/kotlin/runtest.md %}