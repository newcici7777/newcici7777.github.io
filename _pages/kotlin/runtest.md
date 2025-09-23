---
title: runTest runBlocking
date: 2025-09-19
keywords: kotlin, runTest runBlocking
---
## runTest()與this
runTest()函式中，建立一個TestScope()的物件，讓testScope物件成為呼叫block()函式的呼叫者，kotlin稱這是「函式接收者」，會把testScope物件作為this物件傳入block()中。<br>
最後會把testScope新物件作為函式傳回值傳回。
{% highlight kotlin linenos %}
// 簡化版（概念說明用）
fun runTest(block: TestScope.() -> Unit): TestScope {
    // 建立一個TestScope()的物件
    val testScope = TestScope()
    // testScope物件.呼叫block()函式
    testScope.block()
    return testScope
}
{% endhighlight %}

所以印出的this就是TestScope()的物件。
{% highlight kotlin linenos %}
  @Test
  fun coroutin13() = runTest {
    println(this)
  }
{% endhighlight %}
```
TestScope[test started]
```

之前在擴展函式apply()的文章有提到，apply()裡面的this是呼叫apply()的物件。<br>
下面程式中，apply()的this是"Hello world"，呼叫擴展函式的物件。<br>
{% highlight kotlin linenos %}
"Hello world".apply { 
   println(this)
}
{% endhighlight %}
```
Hello world
```

而這邊是建立新的物件，使用新的物件呼叫lambda，this就會指向新的物件，apply()擴展函式會把呼叫者(接收者)，作為傳回值傳回。<br>
印出物件的類別。
```
this::class.simpleName
```
{% highlight kotlin linenos %}
  @Test
  fun test() {
    val result = StringBuilder().apply {
      println("obj = ${this::class.simpleName}")
      append("Hello")
      append(" World!")
    }
    println(result)
  }
{% endhighlight %}

## runTest runBlocking
接下來的測試會使用runTest或runBlocking來測試協程。<br>
使用junit的@Test，來協助測試。<br>
{% highlight kotlin linenos %}
import kotlinx.coroutines.test.runTest
import org.junit.Test

class Test01 {
  @Test
  fun coroutin01() = runTest {
  }
}
{% endhighlight %}

## runBlocking與listof
runBlocking與runTest有類似功能。<br>

但以下程式碼，runTest會自動執行list中的協程，而runBlocking不會自動執行。
{% highlight kotlin linenos %}
@Test
fun coroutin14() = runTest {
  val job = Job()
  val list = listOf(
    launch(job) {
      delay(1000)
      println("list[0] finish")
    },
    launch(job) {
      delay(2000)
      println("list[1] finish")
    })
}
{% endhighlight %}
```
list[0] finish
list[1] finish
```

{% highlight kotlin linenos %}
@Test
fun coroutin14() = runBlocking {
  val job = Job()
  val list = listOf(
    launch(job) {
      delay(1000)
      println("list[0] finish")
    },
    launch(job) {
      delay(2000)
      println("list[1] finish")
    })
}
{% endhighlight %}

呼叫start()函式，才會執行。
{% highlight kotlin linenos %}
@Test
fun coroutin14() = runBlocking {
  val job = Job()
  val list = listOf(
    launch(job) {
      delay(1000)
      println("list[0] finish")
    },
    launch(job) {
      delay(2000)
      println("list[1] finish")
    })
  job.start()
}
{% endhighlight %}
```
list[0] finish
list[1] finish
```