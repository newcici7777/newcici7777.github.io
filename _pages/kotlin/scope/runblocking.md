---
title: runBlocking 
date: 2025-10-14
keywords: Android, Kotlin, coroutines, runBlocking
---
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