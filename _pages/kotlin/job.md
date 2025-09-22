---
title: Job
date: 2025-09-19
keywords: kotlin, job
---
中文有翻譯工作、任務。<br>
## Job 生命周期
### 建立空的Job
{% highlight kotlin linenos %}
val job = Job()
{% endhighlight %}

### 啟動Job
{% highlight kotlin linenos %}
 @Test
fun coroutin10() = runTest {
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
### 加入並啟動子協程
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
### runTest {}等待協程執行完畢
join()代表等待，runTest{}是一個協程，裡面有一個協程child在執行，注意child的爸爸是Job，不是runTest，runTest與Job二者是不相同，runTest與child沒有父子關係，純粹就是<span class="markline">等待協程執行完畢</span>。
```
fun coroutin10() = runTest {
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
fun coroutin10() = runTest {
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
fun coroutin10() = runTest {
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

### Job生命周期圖
1. New 建立: Job建立。
2. Active 運行中: Job.start()啟動。
3. child子協程加入job並啟動。
4. Completing 完成中: child.join()等待子協程完成。
5. Completed 已完成: Job與child子協程都執行完畢。

![img]({{site.imgurl}}/kotlin/job_lifecycle.png)<br>

