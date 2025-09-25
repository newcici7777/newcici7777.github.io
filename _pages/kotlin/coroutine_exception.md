---
title: 協程 exception
date: 2025-09-24
keywords: kotlin, coroutine exception
---
## 不正常的Exception
除了CancellationException以外的Exception，都會讓協程拋出例外，讓子協程與父協程因例外被cancel()，不執行。<br>

## exception與cancel()
當子協程拋出exception，父協程收到exception，接下來會進行協程取消cancel()的動作。<br>

1. 取消子協程
2. 取消它自己的協程
3. 將exception，往上拋給自己的父親。

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

### exception與finally
一個子協程拋出exception會造成其它子協程取消，但取消一定會執行finnaly
{% highlight kotlin linenos %}
  @Test
  fun coroutin06() = runTest {
    val handler = CoroutineExceptionHandler { _, exception ->
      println("exception = $exception")
    }
    val scope = CoroutineScope(Job()).launch(handler) {
      val child1 = launch {
        delay(500)
        throw IllegalArgumentException()
      }
      val child2 = launch {
        try {
          delay(2000)
          println("child2 finish")
        } finally {
          println("child2 finally")
        }
      }
      val child3 = launch {
        try {
          delay(2000)
          println("child3 finish")
        } finally {
          println("child3 finally")
        }
      }
    }
    scope.join()
  }
{% endhighlight %}
```
child2 finally
child3 finally
exception = java.lang.IllegalArgumentException
```

### 多個exception
當多個子協程exception，其它exception都會組合在第1個exception中。

但只能輸出第一個exception。
{% highlight kotlin linenos %}
 val handler = CoroutineExceptionHandler { _, exception ->
   println("exception = $exception")
 }
{% endhighlight %}

其它exception，要特別使用以下語法處理。
{% highlight kotlin linenos %}
${exception.suppressed.contentToString()}
{% endhighlight %}

以下程式碼補捉多個exception，child2、child3在finally拋出Exception，不管是否有取消，都會執行到，若拋出Exception沒寫在finally，會直接被cancel，就不會拋出exception。<br>
{% highlight kotlin linenos %}
 @Test
  fun coroutin06() = runTest {
    val handler = CoroutineExceptionHandler { _, exception ->
      println("exception = $exception")
      println("other exceptions = ${exception.suppressed.contentToString()}")
    }
    val scope = CoroutineScope(Job()).launch(handler) {
      val child1 = launch {
        delay(500)
        throw IllegalArgumentException()
      }
      val child2 = launch {
        try {
          delay(2000)
          println("child2 finish")
        } finally {
          throw IndexOutOfBoundsException()
        }
      }
      val child3 = launch {
        try {
          delay(2000)
          println("child3 finish")
        } finally {
          throw ArithmeticException()
        }
      }
    }
    scope.join()
  }
{% endhighlight %}
```
exception = java.lang.IllegalArgumentException
other exceptions = [java.lang.ArithmeticException, java.lang.IndexOutOfBoundsException]
```

### 執行順序
以下程式碼會標註1,2,3,4，handler一定是最後執行。<br>
{% highlight kotlin linenos %}
 @Test
  fun coroutin06() = runTest {
    // 4.
    val handler = CoroutineExceptionHandler { _, exception ->
      println("exception = $exception")
      println("other exceptions = ${exception.suppressed.contentToString()}")
    }
    val scope = CoroutineScope(Job()).launch(handler) {
      // 1.
      val child1 = launch {
        delay(500)
        throw IllegalArgumentException()
      }
      val child2 = launch {
        try {
          delay(2000)
          println("child2 finish")
        } finally {
          // 2.
          println("child2 finally")
        }
      }
      val child3 = launch {
        try {
          delay(2000)
          println("child3 finish")
        } finally {
          // 3.
          throw ArithmeticException()
        }
      }
    }
    scope.join()
  }
{% endhighlight %}

## Android建立全域ExceptionHandler
在main目錄下，建立resources目錄，根據以下階層關係，建立三個目錄。
```
main/resources/META-INF/services/
```

對著上面的目錄按滑鼠右鍵，建立檔案<br>
![img]({{site.imgurl}}/kotlin/global_exception1.png)<br>

檔名是kotlinx.coroutines.CoroutineExceptionHandler<br>
![img]({{site.imgurl}}/kotlin/global_exception2.png)<br>

內容為
```
com.example.coroutine.handler.GlobalExceptionHandler
```

自己寫一個GlobalExceptionHandler，格式如下:<br>
{% highlight kotlin linenos %}
package com.example.coroutine.handler

import android.util.Log
import kotlinx.coroutines.CoroutineExceptionHandler
import kotlin.coroutines.CoroutineContext

class GlobalExceptionHandler : CoroutineExceptionHandler {
  override val key = CoroutineExceptionHandler
  override fun handleException(context: CoroutineContext, exception: Throwable) {
    Log.d("cici", "Global exception: $exception")
  }
}
{% endhighlight %}

試著執行以下程式碼，能否補捉到exception。
{% highlight kotlin linenos %}
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    textv = findViewById<TextView>(R.id.textView)
    submit.setOnClickListener {
      GlobalScope.launch {
        Log.d("cici", "click")
        "abc".substring(10)
      }
    }
  }
{% endhighlight %}
```
Global exception: java.lang.StringIndexOutOfBoundsException: length=3; index=10
```