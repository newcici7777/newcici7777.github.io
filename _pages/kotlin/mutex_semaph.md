---
title: Mutex Semaphore Atomic
date: 2025-10-12
keywords: kotlin, Mutex, Semaphore, Atomic
---
## 以下是建立1000個協程
{% highlight kotlin linenos %}
  @Test
  fun coroutine10() = runBlocking<Unit> {
    var count = 0
    List(1000) {
      GlobalScope.launch { count++ }
    }.joinAll()
    println(count)
  }
{% endhighlight %}
```
966
```

## Mutex 鎖
結果居然不是1000。<br>

使用Mutex鎖，讓要存取count變數的1000個協程「們」，都只能排隊存取，無法所有人同時搶同一個資源。<br>

{% highlight kotlin linenos %}
  @Test
  fun coroutine10() = runBlocking<Unit> {
    var count = 0
    val mutex = Mutex()
    List(1000) {
      GlobalScope.launch {
        mutex.withLock { count++ }
      }
    }.joinAll()
    println(count)
  }
{% endhighlight %}
```
1000
```

## Semphore 信號
使用信號來通知協程可以允許修改資源。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine11() = runBlocking<Unit> {
    var count = 0
    val semaphore = Semaphore(permits = 1)
    List(1000) {
      GlobalScope.launch {
        semaphore.withPermit { count++ }
      }
    }.joinAll()
    println(count)
  }
{% endhighlight %}
```
1000
```

## 原子AtomicInteger
{% highlight kotlin linenos %}
  @Test
  fun coroutine12() = runBlocking<Unit> {
    var count = AtomicInteger(0)
    val semaphore = Semaphore(permits = 1)
    List(1000) {
      GlobalScope.launch {
        count.incrementAndGet()
      }
    }.joinAll()
    println(count.get())
  }
{% endhighlight %}
```
1000
```