---
title: withContext 切換執行緒
date: 2025-10-31
keywords: kotlin, withContext
---
## 執行緒
- Dispatchers.IO
- Dispatchers.Default
- Dispatchers.Main

## 協程作用域
因為withContext是suspend 函式，所以要在協程作用域中，才能使用withContext()函式。<br>

Junit的coroutine scope(協程作用域)是runBlocking，只有在runBlocking協程作用域下，就能呼叫suspend()函式。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine13() = runBlocking {
  	// 切換至IO執行緒
    withContext(Dispatchers.IO) {
       // 程式碼
    }
    println(result2)
  }
{% endhighlight %}

### 使用方式1
withContext是suspend函式，要放在suspend函式中。<br>
{% highlight kotlin linenos %}
  suspend fun funName() {
    withContext(Dispatchers.IO) {
      delay(1000)
      println("test")
    }
  }

  @Test
  fun coroutine14() = runBlocking {
    funName()
  }
{% endhighlight %}

### 使用方式2
函式傳回值是withContext
{% highlight kotlin linenos %}
  suspend fun funName() = withContext(Dispatchers.IO) {
      delay(1000)
      println("test")
  }

  @Test
  fun coroutine14() = runBlocking {
    funName()
  }
{% endhighlight %}

### 使用方式3
函式傳回值是協程作用域coroutineScope
{% highlight kotlin linenos %}
  suspend fun funName() = coroutineScope {
    withContext(Dispatchers.IO) {
      delay(1000)
      println("test")
    }
  }

  @Test
  fun coroutine14() = runBlocking {
    funName()
  }
{% endhighlight %}

### 使用方式4
包在協程作用域中(coroutineScope、CoroutineScope、GlobalScope)。
{% highlight kotlin linenos %}
  @Test
  fun coroutine14() = runBlocking {
    coroutineScope {
      withContext(Dispatchers.IO) {
        delay(1000)
        println("test")
      }
    }
  }
{% endhighlight %}

### 使用方式5
包在協程中(launch、async)。
{% highlight kotlin linenos %}
  @Test
  fun coroutine14() = runBlocking {
    launch {
      withContext(Dispatchers.IO) {
        delay(1000)
        println("test")
      }
    }
    println()
  }
{% endhighlight %}

### 使用方式6
在Activity，不能在Test Case。
{% highlight kotlin linenos %}
GlobalScope.launch (Dispatchers.Main) {
  // 切換至io
  withContext (Dispatchers.IO) {
    delay(1000)
    println("io")
  }
  // 切換回 main
  println("main")
}
{% endhighlight %}

## 順序執行
即便result2的delay秒數才500，但也是先執行完result1，才執行result2。
{% highlight kotlin linenos %}
  @Test
  fun coroutine13() = runBlocking<Unit> {
    val result1 = withContext(Dispatchers.IO) {
      delay(1000)
      "result1"
    }
    println(result1)
    val result2 = withContext(Dispatchers.IO) {
      delay(500)
      "result2"
    }
    println(result2)
  }
{% endhighlight %}
```
result1
result2
```

## 非同步執行
非同步，我自己的記法是多個協程同時執行，不用等待其它協程執行完畢。<br>
使用launch包住withContext，就可以變成非同步。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine15() = runBlocking {
    launch {
      val result1 = withContext(Dispatchers.IO) {
        delay(1000)
        "result1"
      }
      println(result1)
    }

    launch {
      val result2 = withContext(Dispatchers.IO) {
        delay(500)
        "result2"
      }
      println(result2)
    }
    println()
  }
{% endhighlight %}
```
result2
result1
```
