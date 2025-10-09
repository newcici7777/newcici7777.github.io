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

以下程式碼，一個是send，一個是receive。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine02() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      channel.send(1)
      println("send1")

      channel.send(2)
      println("send2")

      channel.send(3)
      println("send3")
      channel.close()
    }
    delay(1000)
    println("receive1 = ${channel.receive()}")
    delay(1000)
    println("receive2 = ${channel.receive()}")
    delay(1000)
    println("receive3 = ${channel.receive()}")
  }
{% endhighlight %}
```
receive1 = 1
send1
receive2 = 2
send2
receive3 = 3
send3
```
### channel buffer
capacity是通道可以放資料的大小。<br>
預設是capacity = 0，也就是傳送3筆資料不會一次傳送完畢，另一端要接收，才能再傳送下一筆資料。<br>
capacity = 2，通道大小為2，可以先傳送2筆資料放在通道，但要傳送第3筆資料時，另一端要先接收一筆資料，通道才會有多餘的空間，才能再傳送。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine01() = runBlocking<Unit> {
    val channel = Channel<Int>(capacity = 2)
    launch {
      channel.send(1)
      println("send1")

      channel.send(2)
      println("send2")

      println("send3 暫停suspend")
      channel.send(3)
      println("send3 send")
      channel.close()
    }
    delay(1000)
    println("receive1 = ${channel.receive()}")
    delay(1000)
    println("receive2 = ${channel.receive()}")
    delay(1000)
    println("receive3 = ${channel.receive()}")

  }
{% endhighlight %}
```
send1
send2
send3 暫停suspend
receive1 = 1
send3 send
receive2 = 2
receive3 = 3
```

### 傳送的時間比接收的時間快
即便傳送間隔只花50 ms，但channel會「暫停」等到「接收」了，才會再send，而不是每50 ms送1次。<br>
執行結果是收到的時間相差約1000 ms。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine05() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      channel.send(1)
      println("send1")
      delay(50)
      channel.send(2)
      println("send2")
      delay(50)
      channel.send(3)
      println("send3")
      channel.close()
    }
    val startT = System.currentTimeMillis()
    delay(1000)
    println("receive1 = ${channel.receive()} time = ${System.currentTimeMillis() - startT}")
    delay(1000)
    println("receive2 = ${channel.receive()} time = ${System.currentTimeMillis() - startT}")
    delay(1000)
    println("receive3 = ${channel.receive()} time = ${System.currentTimeMillis() - startT}")
  }
{% endhighlight %}
```
receive1 = 1 time = 1012
send1
receive2 = 2 time = 2029
send2
receive3 = 3 time = 3031
send3
```

即便傳送間隔只花100 ms，但channel會「暫停」等到「接收」了，才會再send，而不是每100 ms送1次。<br>
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

### channel.close() 放在send
channel.close()都是放在傳送完後，要手動關閉通道。
{% highlight kotlin linenos %}
  @Test
  fun coroutine01() = runBlocking<Unit> {
    val channel = Channel<Int>(capacity = 2)
    launch {
      // 傳送的區域
      channel.send(1)
      println("send1")

      channel.send(2)
      println("send2")

      // close放在傳送的區域
      channel.close()
    }
    // 接收的區域
    delay(1000)
    println("receive1 = ${channel.receive()}")
    delay(1000)
    println("receive2 = ${channel.receive()}")

  }
{% endhighlight %}

## channel 迴圈
### channel for
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

### channel iterator
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



## 多個接收者
以下有三個協程，一個協程是傳送者，另外二個是接收者協程。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine03() = runBlocking<Unit> {
    val channel = Channel<Int>()
    launch {
      for (i in 1 .. 5) {
        channel.send(i)
      }
      channel.close()
    }
    // receiver1
    launch {
      for (data in channel) {
        println("receiver1 data = $data")
      }
    }
    // receiver2
    launch {
      for (data in channel) {
        println("receiver2 data = $data")
      }
    }
  }
{% endhighlight %}
```
receiver1 data = 1
receiver1 data = 2
receiver1 data = 4
receiver2 data = 3
receiver1 data = 5
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

## select
### 語法
- onReceive() 在select{}中，接收訊息。
- onSend() 在select{}中，傳遞訊息。

```
select<傳回值型態> {
	多個channel
}
```

### 取得傳送速度最快的通道
select裡面放多個通道。<br>
value（該 Channel 傳來的資料）<br>
onReceive()接收資料。<br>
```
select<傳回值型態> {
	channel1.onReceive {value -> "傳回值" }
	channel2.onReceive {value -> "傳回值" }
	channel3.onReceive {value -> "傳回值" }
}
```

如果有多個Channel，要分辦那個通道先發送消息，使用select。<br>
因為delay(50)的速度比較快，select會先接收到World。<br>
後面加上coroutineContext.cancelChildren()，是因為阻塞，junit無法執行完畢。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine06() = runBlocking<Unit> {
    val channel1 = Channel<String>()
    val channel2 = Channel<String>()
    launch {
      delay(100)
      channel1.send("Hello")
      channel1.close()
    }
    launch {
      delay(50)
      channel2.send("World")
      channel2.close()
    }
    val result = select<String> {
      channel1.onReceive { value ->
        "channel1 data = $value"
      }
      channel2.onReceive { value ->
        "channel2 data = $value"
      }
    }
    println("result = $result")
    coroutineContext.cancelChildren()
  }
{% endhighlight %}
```
result = channel2 data = World
```

### 判斷那個通道的buffer未滿
判斷那個通道的buffer未滿，就用那個通道傳送資料。<br>

onSend語法
```
select<傳回值型態> {
	channel1.onSend(傳送給通道的值) { 傳送成功的傳回值callback }
	channel2.onSend("World") {"傳回值"}
}
```

以下程式碼是判斷那個通道有空間，就送那邊。<br>
以下Unit是沒有傳回值。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine07() = runBlocking<Unit> {
    val channel1 = Channel<String>(capacity = 1)
    val channel2 = Channel<String>(capacity = 1)
    // channel1先塞滿資料
    channel1.send("Hello")

    select<Unit> {
      channel1.onSend("ABC") {
        println("channel1 send")
      }
      channel2.onSend("World") {
        println("channel2 send")
      }
    }
    coroutineContext.cancelChildren()
  }
{% endhighlight %}
```
channel2 send
```

有傳回值的寫法。
{% highlight kotlin linenos %}
  @Test
  fun coroutine07() = runBlocking<Unit> {
    val channel1 = Channel<String>(capacity = 1)
    val channel2 = Channel<String>(capacity = 1)
    // channel1先塞滿資料
    channel1.send("Hello")

    val result = select<String> {
      channel1.onSend("ABC") {
        "channel1 send"
      }
      channel2.onSend("World") {
        "channel2 send"
      }
    }
    println(result)
    coroutineContext.cancelChildren()
  }
{% endhighlight %}
```
channel2 send
```

### 判斷那一個job先完成onAwait
使用有傳回值的async，不能是launch，launch沒有傳回值。<br>
async的傳回值Deferred要用onAwait()來接收。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine08() = runBlocking<Unit> {
    val job1 = async {
      delay(50)
      "job1 finish"
    }
    val job2 = async {
      delay(10)
      "job2 finish"
    }

    val result = select<String> {
      job1.onAwait { it }
      job2.onAwait { it }
    }
    println(result)
    coroutineContext.cancelChildren()
  }
{% endhighlight %}
```
job2 finish
```
### 判斷那一個job先完成onJoin
使用launch，launch沒有傳回值。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine09() = runBlocking<Unit> {
    val job1 = launch {
      delay(50)
      println("job1 finish")
    }
    val job2 = launch {
      delay(10)
      println("job2 finish")
    }

    select<Unit> {
      job1.onJoin { println("select job1 finish") }
      job2.onJoin { println("select job2 finish") }
    }
    coroutineContext.cancelChildren()
  }
{% endhighlight %}
```
job2 finish
select job2 finish
```

### 比較

| 類型                     | 監聽事件  | 說明                    |
| :---- | :---- | :---- |
| `channel.onReceive {}` | 收資料   | 哪個 Channel 先有資料就先處理哪個 |
| `channel.onSend {}`    | 傳資料   | 哪個 Channel 有空間就先發送    |
| `deferred.onAwait {}`  | 等結果   | 哪個任務先完成就先用誰           |
| `job.onJoin {}`        | 等任務結束 | 等待最早結束的任務             |
