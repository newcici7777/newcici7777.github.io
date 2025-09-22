---
title: cancel
date: 2025-09-22
keywords: kotlin, Coroutine cancel
---
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

### delay()
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
