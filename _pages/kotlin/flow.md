---
title: flow
date: 2025-09-26
keywords: kotlin, flow
---
flow是有順序的把管子中的資料一個一個發射(emit)出去，接收端也是一個一個接收(collect)資料。<br>

資料不是同時發射，也不是同時接收，皆是有順序性的發射與接收。<br>

什麼時候發射(emit)資料，當呼叫collect時，才會發射(emit)資料。<br>

## import
```
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*
```

## 簡單flow程式
當flow呼叫collect()時，才會執行flowtest1()。<br>
使用emit()每一秒發射一筆資料，而collect()就會一筆一筆接收資料，接收到資料就會輸出在螢幕。<br>
{% highlight kotlin linenos %}
fun flowtest1() = flow<Int> {
  for (i in 1..3) {
    delay(1000)
    emit(i)
  }
}
@Test
fun coroutine10() = runBlocking {
  val flow = flowtest1()
  println("call collect..")
  flow.collect { value ->
    println(value)
  }
}
{% endhighlight %}
```
call collect..
1
2
3
```
## 證明flow與launch是同時執行
coroutine10()中有launch協程與flow流同時啟動，並同時執行。<br>
可以看到執行結果互相交叉輸出。<br>
{% highlight kotlin linenos %}
fun flowtest1() = flow<Int> {
  for (i in 1..3) {
    delay(1000)
    emit(i)
  }
}
@Test
fun coroutine10() = runBlocking {
  launch {
    for (i in 10 .. 15) {
      delay(1000)
      println("other job i = $i")
    }
  }
  val flow = flowtest1()
  println("call collect..")
  flow.collect { value ->
    println(value)
  }
}
{% endhighlight %}
```
call collect..
1
other job i = 10
2
other job i = 11
3
other job i = 12
other job i = 13
other job i = 14
other job i = 15
```
## 多次collect
再一次collect就重新再發射(emit)一次資料。
{% highlight kotlin linenos %}
fun flowtest1() = flow<Int> {
  for (i in 1..3) {
    delay(1000)
    emit(i)
  }
}
@Test
fun coroutine11() = runBlocking {
  val flow = flowtest1()
  println("call collect..")
  flow.collect { value ->
    println(value)
  }
  println("collect..again")
  flow.collect { value ->
    println(value)
  }
}
{% endhighlight %}
```
call collect..
1
2
3
collect..again
1
2
3
```

## asFlow()
asFlow()本身就有emit()的功能，asFlow使用在陣列或集合。<br>
以下是asFlow()的原始碼，在forEach{}之間，一個一個發射(emit)。<br>
{% highlight kotlin linenos %}
public fun IntRange.asFlow(): Flow<Int> = flow {
    forEach { value ->
        emit(value)
    }
}
{% endhighlight %}

實際使用方式。<br>
{% highlight kotlin linenos %}
@Test
fun coroutine12() = runBlocking<Unit> {
  (1..3).asFlow().collect{
    println("data = $it")
  }
}
{% endhighlight %}
```
data = 1
data = 2
data = 3
```

## filter
asFlow()結合filter(過濾器)，僅保留符合條件的元素。
```
filter { 條件 }
```
以下程式碼，若除2的餘數不為0，也就是奇數，就會保留下來，偶數就會剔除掉。<br>
{% highlight kotlin linenos %}
@Test
fun coroutine12() = runBlocking<Unit> {
  (1..3).asFlow().filter { it % 2 != 0 }.collect{
    println("data = $it")
  }
}
{% endhighlight %}
```
data = 1
data = 3
```
## flowOf
flowOf(參數是可變集合)，原始碼針對每一個元素進行發射。<br>
{% highlight kotlin linenos %}
public fun <T> flowOf(vararg elements: T): Flow<T> = flow {
    for (element in elements) {
        emit(element)
    }
}
{% endhighlight %}

實際應用如下，字串陣列abc、def、ghi，逐一發射(emit)與接收(collect)。
{% highlight kotlin linenos %}
@Test
fun coroutine13() = runBlocking<Unit> {
  flowOf("abc", "def", "ghi").collect {
    println("data = $it")
  }
}
{% endhighlight %}
```
data = abc
data = def
data = ghi
```

## 同一個執行緒與context
flow從emit()到collect()都是在同一個Context，同一個執行緒。<br>
{% highlight kotlin linenos %}
fun flowtest2() = flow<Int> {
  println("Flow Context = ${Thread.currentThread().name}")
  for (i in 1..3) {
    delay(1000)
    emit(i)
  }
}
@Test
fun coroutine14() = runBlocking<Unit> {
  flowtest2().collect { value ->
    println("Collected $value ${Thread.currentThread().name}")
  }
}
{% endhighlight %}
```
Flow Context = Test worker @coroutine#1
Collected 1 Test worker @coroutine#1
Collected 2 Test worker @coroutine#1
Collected 3 Test worker @coroutine#1
```

## flowOn
flowOn可以改變context。<br>
假設要從網路下載圖片，要使用調度器IO，而UI是在Main Thread，不能互相影嚮，所以需要flowOn，來分成二個不同的執行緒，一個是main Thread，一個是background work thread。<br>
### 錯誤的改變context
把flowtest2()調度器改成IO(硬碟輸出入、網路存取、資料庫存取)。<br>
但執行完會有error，系統會告知你二個是不同的協程，不同的context。<br>
{% highlight kotlin linenos %}
fun flowtest2() = flow<Int> {
  withContext(Dispatchers.IO) {
    println("Flow Context = ${Thread.currentThread().name}")
    for (i in 1..3) {
      delay(1000)
      emit(i)
    }
  }
}
@Test
fun coroutine14() = runBlocking<Unit> {
  flowtest2().collect { value ->
    println("Collected $value ${Thread.currentThread().name}")
  }
}
{% endhighlight %}
```
Flow invariant is violated:
		Flow was collected in [CoroutineId(1), "coroutine#1":BlockingCoroutine{Active}@26e1a71d, BlockingEventLoop@6e8fc98b],
		but emission happened in [CoroutineId(1), "coroutine#1":DispatchedCoroutine{Active}@28717622, Dispatchers.IO]
```

### 正確使用flowOn
使用flowOn，可以發現flowtest2()使用的是Thread是`DefaultDispatcher-worker-1 @coroutine#2`，而coroutine14()使用的Thread是`Test worker @coroutine#1`。<br>
{% highlight kotlin linenos %}
fun flowtest2() = flow<Int> {
  println("Flow Context = ${Thread.currentThread().name}")
  for (i in 1..3) {
    delay(1000)
    emit(i)
  }
}.flowOn(Dispatchers.IO)

@Test
fun coroutine14() = runBlocking<Unit> {
  flowtest2().collect { value ->
    println("Collected $value ${Thread.currentThread().name}")
  }
}
{% endhighlight %}
```
Flow Context = DefaultDispatcher-worker-1 @coroutine#2
Collected 1 Test worker @coroutine#1
Collected 2 Test worker @coroutine#1
Collected 3 Test worker @coroutine#1
```

## onEach
設定每一個元素要做什麼事。<br>
以下程式碼設定每個元素都要暫停1秒，再發射。<br>
執行結果，每筆資料的時間相差約1000ms。<br>
{% highlight kotlin linenos %}
@Test
fun coroutine13() = runBlocking<Unit> {
  flowOf("abc", "def", "ghi")
    .onEach { delay(1000) }
    .collect {
      val time = System.currentTimeMillis()
    println("time = $time  data = $it")
  }
}
{% endhighlight %}
```
time = 1759461086051  data = abc
time = 1759461087068  data = def
time = 1759461088070  data = ghi
```
### onEach沒有collect功能
以下程式碼，若只有呼叫onEach()，最後沒有用collect()，就算資料發射，也沒有人去接收。
{% highlight kotlin linenos %}
  fun events() = (1 .. 3)
    .asFlow().flowOn(Dispatchers.IO)
  @Test
  fun coroutine15() = runBlocking<Unit> {
    events()
      .onEach { event ->
        println("Event: $event ${Thread.currentThread().name}") }
      .collect {}
  }
{% endhighlight %}
```
Event: 1 Test worker @coroutine#1
Event: 2 Test worker @coroutine#1
Event: 3 Test worker @coroutine#1
```

## asFlow()指派給函式
以下events()函式，指派`=` (1..3).asFlow() 給函式。<br>
onEach印出event()發射方(emit)的Thread。<br>
{% highlight kotlin linenos %}
fun events() =
  (1..3).asFlow().onEach { event ->
    println("events() $event ${Thread.currentThread().name}")
  }.flowOn(Dispatchers.IO)
{% endhighlight %}

coroutine15()中的onEach印出接收方(collect)的thared。<br>
真正接收資料是collect()函式。<br>
{% highlight kotlin linenos %}
@Test
fun coroutine15() = runBlocking<Unit> {
  events()
    .onEach { event ->
      println("coroutine15(): $event ${Thread.currentThread().name}")
    }
    .collect { value ->
      println("data= $value ${Thread.currentThread().name}")
    }
}
{% endhighlight %}
```
events() 1 DefaultDispatcher-worker-1 @coroutine#2
events() 2 DefaultDispatcher-worker-1 @coroutine#2
events() 3 DefaultDispatcher-worker-1 @coroutine#2
coroutine15(): 1 Test worker @coroutine#1
data= 1 Test worker @coroutine#1
coroutine15(): 2 Test worker @coroutine#1
data= 2 Test worker @coroutine#1
coroutine15(): 3 Test worker @coroutine#1
data= 3 Test worker @coroutine#1
```
## launchIn()改變Scope
launchIn()可以改變Scope。<br>
this代表目前的Thread。<br>
```
launchIn(this)
```
{% highlight kotlin linenos %}
fun events() =
  (1..3).asFlow().onEach { event ->
    println("events() $event ${Thread.currentThread().name}")
  }.flowOn(Dispatchers.IO)

@Test
fun coroutine15() = runBlocking<Unit> {
  events()
    .onEach { event ->
      println("coroutine15(): $event ${Thread.currentThread().name}")
    }.launchIn(this)
}
{% endhighlight %}
```
events() 1 DefaultDispatcher-worker-1 @coroutine#3
events() 2 DefaultDispatcher-worker-1 @coroutine#3
events() 3 DefaultDispatcher-worker-1 @coroutine#3
coroutine15(): 1 Test worker @coroutine#2
coroutine15(): 2 Test worker @coroutine#2
coroutine15(): 3 Test worker @coroutine#2
```

### scope
因為變成別的scope，所以最後要使用join()，這樣runBlocking的Scope才會等待CoroutineScope結束才結束，不然印不出來結果。
```
.launchIn(CoroutineScope(Dispatchers.Default))
.join()
```
{% highlight kotlin linenos %}
  fun events() =
    (1..3).asFlow().onEach { event ->
      println("events() $event ${Thread.currentThread().name}")
    }.flowOn(Dispatchers.IO)

  @Test
  fun coroutine15() = runBlocking<Unit> {
    events()
      .onEach { event ->
        println("coroutine15(): $event ${Thread.currentThread().name}")
      }.launchIn(CoroutineScope(Dispatchers.Default))
      .join()
  }
{% endhighlight %}

### launchIn 傳回值
launchIn()會傳回Job，因此可以去接收傳回值。
{% highlight kotlin linenos %}
public fun <T> Flow<T>.launchIn(scope: CoroutineScope): Job = scope.launch {
    collect() // tail-call
}
{% endhighlight %}

以下程式碼，接收launchIn()的傳回值Job，並且在2.5秒取消協程。<br>
onEach設為1秒發射(emit)一次。<br>
結果只有接收2筆。<br>
{% highlight kotlin linenos %}
  fun events() =
    (1..3).asFlow().onEach {
      delay(1000)
    }.flowOn(Dispatchers.IO)

  @Test
  fun coroutine15() = runBlocking<Unit> {
    val job = events()
      .onEach { event ->
        val time = System.currentTimeMillis()
        println("event(): $event time = $time")
      }.launchIn(CoroutineScope(Dispatchers.Default))
    delay(2500)
    job.cancel()
    job.join()
  }
{% endhighlight %}
```
event(): 1 time = 1759476638927
event(): 2 time = 1759476639932
```

## Cancel Flow
flow發射emit有使用ensureActive來確認collect接收方是否已經取消接收。
### cancel()
收集資料到i == 3，就cancel flow。
{% highlight kotlin linenos %}
  fun testFlow5() = flow<Int> {
    for (i in 1..5) {
      delay(1000)
      emit(i)
      println("emit = $i")
    }
  }
  @Test
  fun coroutine18() = runBlocking<Unit> {
    testFlow5().collect { value ->
      println(value)
      // 收集資料到i == 3，就cancel flow。
      if (value == 3) cancel()
    }
  }
{% endhighlight %}
```
1
emit = 1
2
emit = 2
3
emit = 3

BlockingCoroutine was cancelled
```
### withTimeoutOrNull
testFlow4()每一秒發射一次，發完1到3需要3秒鐘，但是`withTimeoutOrNull(2500)`代表2.5秒後會被取消協程，所以執行結果只有發射2筆。<br>
{% highlight kotlin linenos %}
  fun testFlow4() = flow<Int> {
    for (i in 1 .. 3) {
      delay(1000)
      emit(i)
      println("emit = $i")
    }
  }

  @Test
  fun coroutine17() = runBlocking<Unit> {
    withTimeoutOrNull(2500) {
      testFlow4().collect { value ->
        println(value)
      }
    }
  }
{% endhighlight %}
```
1
emit = 1
2
emit = 2
```
### asFlow cancellable
asFlow的cancel一定要加上cancellable()，因為asFlow是一次性的發射(emit)，不用cancellable無法使用cancel()
{% highlight kotlin linenos %}
  @Test
  fun coroutine16() = runBlocking<Unit> {
    (1..5).asFlow().cancellable().collect { value ->
      println(value)
      if (value == 3) cancel()
    }
  }
{% endhighlight %}
```
1
2
3

BlockingCoroutine was cancelled
kotlinx.coroutines.JobCancellationException: BlockingCoroutine was cancelled; job=nul
```
## buffer
發射的時間比較快，每發射一筆資料要花100 ms，但接收的時間比較慢，每接收一筆要花200ms。<br>
如何使用buffer讓每一筆資料是同時發射呢？<br>

### flow emit是依序執行
所謂emit依序執行，就是發射完一個，再發射下一個，以下程式碼計算emit發射的時間，發射5次，總共為500ms 左右。<br>
但接收端仍是依序接收，接收每一次就暫停200 ms，接收5次就暫停1000 ms。<br>
總共花費時間約1500 ms。<br>
{% highlight kotlin linenos %}
  fun testFlow6() = flow<Int> {
    for (i in 1..5) {
      delay(100)
      emit(i)
      println("emit = $i")
    }
  }

  @Test
  fun coroutine19() = runBlocking<Unit> {
    val time = measureTimeMillis {
      testFlow6().collect { value ->
        delay(200)
        println(value)
      }
    }
    println("total emit time = $time ms")
  }
{% endhighlight %}
```
1
emit = 1
2
emit = 2
3
emit = 3
4
emit = 4
5
emit = 5
total emit time = 1573 ms
```

### flow buffer()同時發射
接收端每200秒接收一次，使用buffer(大小)，會讓emit同時發射，同時發射5筆只會使用100 ms。<br>
但接收端仍是依序接收，接收每一次就暫停200 ms，接收5次就暫停1000 ms。<br>
所以總共花費時間約1100 ms。
{% highlight kotlin linenos %}
  fun testFlow6() = flow<Int> {
    for (i in 1..5) {
      delay(100)
      emit(i)
      println("emit = $i")
    }
  }
  @Test
  fun coroutine20() = runBlocking<Unit> {
    val time = measureTimeMillis {
      testFlow6().buffer(50).collect { value ->
        delay(200)
        println(value)
      }
    }
    println("total emit time = $time ms")
  }  
{% endhighlight %}
```
emit = 1
emit = 2
1
emit = 3
emit = 4
2
emit = 5
3
4
5
total emit time = 1169 ms
```
### 與flowOn的區別
buffer不會改變發射方的Thread，發射跟接收都是在同一個Thread。<br>
而先前的flowOn()，使發射方可以與接收方同時進行，而不是依序執行，發射方的Thread與接收方的Thread是不同的。<br>
以下程式執行時間也是約1100 ms，但使用的原理是不同。<br>
{% highlight kotlin linenos %}
  fun testFlow6() = flow<Int> {
    for (i in 1..5) {
      delay(100)
      emit(i)
      println("emit = $i")
    }
  }
  @Test
  fun coroutine21() = runBlocking<Unit> {
    val time = measureTimeMillis {
      testFlow6().flowOn(Dispatchers.Default).collect { value ->
        delay(200)
        println(value)
      }
    }
    println("total emit time = $time ms")
  }
{% endhighlight %}
```
emit = 1
emit = 2
1
emit = 3
emit = 4
2
emit = 5
3
4
5
total emit time = 1159 ms
```

## 改變上游
Flow(流)，分為上游與下游，上游是指發射端emit的部分，下游是指接收端collect的部分。<br>

### transform
transform可以改變上游，讓發射端，發射多筆。<br>