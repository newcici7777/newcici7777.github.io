---
title: Channel 通道
date: 2025-10-08
keywords: kotlin, Channel
---
Channel跟flow一樣，有傳送資料與接收資料的功能。<br>

Channel的傳送資料是send()，接收資料是receive()。<br>

儲存要傳送資料的地方，稱為Channel。<br>

Channel跟Flow一樣都是順序(依序)傳送。<br>

||Flow|Channel|
|:---:|:---:|:---:|
|傳送|emit()|send()|
|接收|collect()|receive()|
|傳送資料的地方|Flow 資料流|Channel 通道|

## Channel 語法
```
val 變數 = Channel<資料型態>()
val chanel = Channel<Int>()
```

傳送
```
chanel.send(1)
chanel.send("Test")
```

接收
```
val data = chanel.receive()
```

以下程式碼中有二個協程，一個是send，一個是receive，但因為無法判斷接收是否完畢，不建議用以下程式碼。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine16() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      for (i in 1..5) {
        delay(1000)
        channel.send(i)
      }
      channel.close()
    }
    launch {
      while (true) {
        val data = channel.receive()
        println("data = $data")
      }
    }
  }
{% endhighlight %}
```
data = 1
data = 2
data = 3
data = 4
data = 5

Channel was closed
kotlinx.coroutines.channels.ClosedReceiveChannelException: Channel was closed
```

## channel for
使用for代替receive。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine16() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      for (i in 1..5) {
        delay(1000)
        channel.send(i)
      }
      channel.close()
    }
    launch {
      for (data in channel) {
        println("data = $data")
      }
    }
  }
{% endhighlight %}
```
data = 1
data = 2
data = 3
data = 4
data = 5
```

## channel iterator
{% highlight kotlin linenos %}
  @Test
  fun coroutine16() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      for (i in 1..5) {
        delay(1000)
        channel.send(i)
      }
      channel.close()
    }
    launch {
      val iter = channel.iterator()
      while (iter.hasNext()) {
        val data = iter.next()
        println("data = $data")
      }
    }
  }
{% endhighlight %}
```
data = 1
data = 2
data = 3
data = 4
data = 5
```

## 傳送的時間比接收的時間快
即便傳送只花100 ms，但channel會「暫停」等到「接收」了，才會再send，而不是每100 ms送1次。<br>
執行結果是收到的時間相差約1000 ms。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine17() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      for (i in 1..5) {
        delay(100)
        channel.send(i)
      }
      channel.close()
    }
    launch {
      val startT = System.currentTimeMillis()
      for (data in channel) {
        delay(1000)
        println("data = $data time = ${System.currentTimeMillis() - startT} ")
      }
    }
  }
{% endhighlight %}
```
data = 1 time = 1105 
data = 2 time = 2125 
data = 3 time = 3126 
data = 4 time = 4131 
data = 5 time = 5135 
```
## produce與actor
produce與actor自動建立傳送訊息的channel通道，而且會自動關閉channel，不用手動關閉close。
### produce 產生訊息
{% highlight kotlin linenos %}
  fun produceNumber() = GlobalScope.produce<Int> {
    for (i in 1..5) {
      delay(1000)
      send(i)
    }
  }
  @Test
  fun coroutine18() = runBlocking<Unit> {
    val channel = produceNumber()
    for (data in channel) {
      println("data = $data")
    }
  }
{% endhighlight %}
```
data = 1
data = 2
data = 3
data = 4
data = 5
```
### actor 接收訊息
{% highlight kotlin linenos %}
  fun receiver() = GlobalScope.actor<Int> {
    for (data in channel) {
      println("data = $data")
    }
  }
  @Test
  fun coroutine19() = runBlocking<Unit> {
    val channel = receiver()
    for (i in 1..5) {
      channel.send(i)
    }
  }
{% endhighlight %}

## 判斷Channel是否關閉
傳送端用isClosedForSend。<br>
接收端用isClosedForReceive。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine21() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      for (i in 1..5) {
        delay(1000)
        channel.send(i)
      }
      channel.close()
      println("isClosedForSend = ${channel.isClosedForSend}")
    }
    launch {
      for (data in channel) {
        println("data = $data")
      }
      println("isClosedForReceive = ${channel.isClosedForReceive}")
    }
  }
{% endhighlight %}

### receiveCatching
邊檢查channel是否關閉，邊取出channel資料。<br>
傳回值透過isClosed，判斷已關閉。<br>
傳回值透過getOrNull()，取出channel中的資料。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine16() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      for (i in 1..5) {
        delay(1000)
        channel.send(i)
      }
      channel.close()
    }
    launch {
      while (true) {
        val result = channel.receiveCatching()
        if (result.isClosed) break
        println("data = ${result.getOrNull()}")
      }
    }
  }
{% endhighlight %}
```
data = 1
data = 2
data = 3
data = 4
isClosedForSend = true
data = 5
isClosedForReceive = true
```