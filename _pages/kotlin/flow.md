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

## launchIn()改變Scope
除了改變Context之外，launchIn()可以改變Scope。
