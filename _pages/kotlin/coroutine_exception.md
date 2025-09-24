---
title: 協程 exception
date: 2025-09-24
keywords: kotlin, coroutine exception
---
## 根協程 例外
### launch
launch就直接拋出例外。<br>
launch需要join()。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin26() = runTest {
    val job = GlobalScope.launch {
      throw IndexOutOfBoundsException()
    }
    job.join()
  }
{% endhighlight %}
### async
async需要透過傳回值await()，才可以補捉到例外。<br>
async的await()本身就有join()的功能，會請runTest等待到job1執行完畢才退出。<br>
{% highlight kotlin linenos %}
@Test
fun coroutin27() = runTest {
  val job1 = GlobalScope.async {
    throw ArithmeticException()
  }
  try {
    job1.await()
  } catch (e: Exception) {
    e.printStackTrace()
  }
}
{% endhighlight %}

## 非根協程 協程中的協程例外
如果是協程中的協程產生例外，不用使用傳回值await()，直接拋出例外。
{% highlight kotlin linenos %}
  @Test
  fun coroutin28() = runTest {
    val job = GlobalScope.launch {
      val child = async {
        throw IllegalArgumentException()
      }
    }
    job.join()
  }
{% endhighlight %}
```
Exception in thread "DefaultDispatcher-worker-2 @coroutine#3" java.lang.IllegalArgumentException
```

## SupervisorJob() 與Job()
### Job()
作用域的參數是Job()
```
CoroutineScope(Job())
```
子協程有例外，其它子協程也會被取消。
{% highlight kotlin linenos %}
  @Test
  fun coroutin01() = runTest {
    val scope = CoroutineScope(Job())
    val job1 = scope.launch {
      delay(100)
      throw IllegalArgumentException()
    }
    val job2 = scope.launch {
      delay(1000)
      println("job2 finish")
    }
    joinAll(job1, job2)
  }
}
{% endhighlight %}
```
java.lang.IllegalArgumentException
```

### SupervisorJob()
作用域的參數是SupervisorJob()<br>
```
CoroutineScope(SupervisorJob())
```
子協程有例外，其它子協程也會執行，以下執行結果，會顯示job2 finish。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin01() = runTest {
    val scope = CoroutineScope(SupervisorJob())
    val job1 = scope.launch {
      delay(100)
      throw IllegalArgumentException()
    }
    val job2 = scope.launch {
      delay(1000)
      println("job2 finish")
    }
    joinAll(job1, job2)
  }
}
{% endhighlight %}
```
job2 finish

java.lang.IllegalArgumentException
```


## coroutineScope supervisorScope Exception
- coroutinScope 子協程失敗，其它子協程全取消
- supervisorScope 子協程失敗，不會影嚮其它子協程
- supervisorScope 父協程失敗，其它子協程全取消

### coroutineScope Exception
job2 拋出Exception()，導致job1被cancel()，不會執行job1 finish。
{% highlight kotlin linenos %}
  fun coroutin06() = runTest {
    coroutineScope {
      val job1 = launch {
        delay(400)
        println("job1 finish")
      }
      val job2 = async{
        delay(200)
        println("job2 finish")
        throw IllegalArgumentException()
      }
    }
  }
{% endhighlight %}
```
job2 finish

java.lang.IllegalArgumentException
  at com.example.coroutine.Test01$coroutin06$1$1$job2$1.invokeSuspen
```
### supervisorScope 子協程Exception
子協程job2 拋出Exception()，子協程job1仍會執行完畢。
{% highlight kotlin linenos %}
  fun coroutin06() = runTest {
    supervisorScope {
      val job1 = launch {
        delay(400)
        println("job1 finish")
      }
      val job2 = async{
        delay(200)
        println("job2 finish")
        throw IllegalArgumentException()
      }
    }
  }
{% endhighlight %}
```
job2 finish
job1 finish
```

### supervisorScope 父協程Exception
父協程失敗，其它子協程全取消。<br>
child1、child2不會執行。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutin02() = runTest {
    supervisorScope {
      val child1 = launch {
        println("child1")
      }
      val child2 = launch {
        println("child2")
      }
      // 這是父協程supervisorScope的Exception
      throw IllegalArgumentException()
    }
  }
}
{% endhighlight %}
```
java.lang.IllegalArgumentException
```

## CoroutineExceptionHandler
只能補捉launch的Exception。<br>

只能在supervisorScope、GlobalScope()、CoroutineScope()根協程下補捉作用域中所有Exception、包含作用域下子協程的例外。<br>

### GlobalScope
{% highlight kotlin linenos %}
@Test
fun coroutin03() = runTest {
  val handler = CoroutineExceptionHandler{ _, exception ->
    println("exception = $exception")
  }
  val scope = GlobalScope.launch(handler){
    throw IllegalArgumentException()
  }
  scope.join()
}
{% endhighlight %}
```
exception = java.lang.IllegalArgumentException
```

### CoroutineScope
{% highlight kotlin linenos %}
@Test
fun coroutin04() = runTest {
  val handler = CoroutineExceptionHandler{ _, exception ->
    println("exception = $exception")
  }
  val scope = CoroutineScope(Job()).launch(handler){
    throw IllegalArgumentException()
  }
  scope.join()
}
{% endhighlight %}
```
exception = java.lang.IllegalArgumentException
```

### handler補捉子協程例外
父協程的launch(handler)，子協程有Exception也會補捉到Exception
```
CoroutineScope(Job()).launch(handler)
```
{% highlight kotlin linenos %}
@Test
fun coroutin04() = runTest {
  val handler = CoroutineExceptionHandler{ _, exception ->
    println("exception = $exception")
  }
  val scope = CoroutineScope(Job()).launch(handler){
    val child = launch {
      throw IllegalArgumentException()
    }
  }
  scope.join()
}
{% endhighlight %}

### handler無法補捉子協程例外
子協程launch(handler)，handler無法補捉到Exception
```
val child = launch(handler) 
```
{% highlight kotlin linenos %}
@Test
fun coroutin04() = runTest {
  val handler = CoroutineExceptionHandler{ _, exception ->
    println("exception = $exception")
  }
  val scope = CoroutineScope(Job()).launch{
    val child = launch(handler) {
      throw IllegalArgumentException()
    }
  }
  scope.join()
}
{% endhighlight %}