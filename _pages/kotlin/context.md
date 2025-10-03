---
title: CoroutineContext
date: 2025-09-25
keywords: kotlin, CoroutineContext
---
CoroutineContext，協程上下文，由以下四個元素所組成。<br>

|元素類型|作用|範例|
|Job|管理協程生命週期|Job()|
|Dispatcher|決定執行緒|Dispatchers.IO、Dispatchers.Main|
|CoroutineName|協程名稱（除錯用）|CoroutineName("MyCoroutine")|
|CoroutineExceptionHandler|異常處理|CoroutineExceptionHandler|

## 輸出上下文
使用時記得要加上大括號。
{% highlight kotlin linenos %}
${coroutineContext[Job]}
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
fun coroutin07() = runBlocking {
  println("job = ${coroutineContext[Job]}")
  println("dispatcher = ${coroutineContext[CoroutineDispatcher]}")
  println("name = ${coroutineContext[CoroutineName]}")
  // 獲取特定元素
  val job = coroutineContext.job
  println("協程Job: $job")
}
{% endhighlight %}
```
job = TestScope[test started]
dispatcher = StandardTestDispatcher[scheduler=kotlinx.coroutines.test.TestCoroutineScheduler@1b7c473a]
name = null
協程Job: TestScope[test started]
```

## 調度器Dispatchers
Dispatchers調度器決定協程是在那個Thread運行。<br>
預設是目前runBlocking{}的Thread。<br>

Android的Main Thread是處理UI。<br>

Dispatchers分類如下:<br>
- Dispatchers.Main 畫面UI
- Dispatchers.IO 網路傳輸、資料庫、File存取
- Dispatchers.Default Cpu運算

{% highlight kotlin linenos %}
  @Test
  fun coroutin08() = runBlocking {
    launch {
      println("main Thread = ${Thread.currentThread().name}")
    }

    launch(Dispatchers.IO){
      println("IO Thread = ${Thread.currentThread().name}")
    }

    launch(Dispatchers.Default){
      println("Default Thread = ${Thread.currentThread().name}")
    }

    launch (newSingleThreadContext("MyThread")) {
      println("Custom Thread = ${Thread.currentThread().name}")
    }
  }
{% endhighlight %}

## 自訂CoroutineContext
- Job() 自己建立一個job物件
- Dispatchers.IO 調度器是IO
- CoroutineName名字是test

輸出CoroutineContext與Thread name
{% highlight kotlin linenos %}
fun coroutine09() = runBlocking {
  val scope = CoroutineScope(Job() + Dispatchers.IO
      + CoroutineName("test"))
  val job1 = scope.launch {
    println("${coroutineContext[Job]}")
    println("${Thread.currentThread().name}")
  }
  job1.join()
}
{% endhighlight %}
```
"test#3":StandaloneCoroutine{Active}@5a5ff9a1
DefaultDispatcher-worker-1 @test#3
```

CoroutineName名字
```
名字#編號
test#3
```

物件名
```
@5a5ff9a1
```