---
title: Okhttp
date: 2025-10-23
keywords: kotlin, Okhttp
---
Prerequisites:

- [withContext][1]

## 同步 非同步 get post

|類型|	特點|	使用方法|
|:------|:------------|:--------|
|同步 (Synchronous)|	會阻塞目前執行緒，直到伺服器回應|	call.execute()|
|非同步 (Asynchronous)|	不阻塞執行緒，回應會在callback 回呼中收到	|call.enqueue(callback)|

所謂的阻塞(Blocking)，就是要等待網路連線回應，才進行下一個程式碼執行。<br>

非同步，就是不用等待網路連線，可以先執行其它程式碼，不用卡在那邊等待網路回應。<br>

### 同步 get
{% highlight kotlin linenos %}
import okhttp3.OkHttpClient
import org.junit.Test

import okhttp3.Request
class OkhttpTest {
  val client = OkHttpClient()
  @Test
  fun sync_get() {
    val request = Request.Builder()
      .url("https://www.httpbin.org/get")
      .get()
      .build()
    val call = client.newCall(request)
    val response = call.execute()
    if (response.isSuccessful) {
      response.body().let { body ->
        println("body = ${body?.string()}")
      }
    }
  }
}
{% endhighlight %}

### 非同步 get
Kotlin 避免回調地獄，改用協程取代call.enqueue()。<br>
我在這邊使用[withContext][1]，也可以用launch \+ withContext，達到非同步。<br>

另外注意！`body?.string()`只能呼叫一次，不能呼叫2次，要使用一個變數，把它存起來，呼叫2次會出現以下的錯誤，因為讀取第一次時資源就關閉了，讀第2次，就會有closed的Exception，因為資源已關閉。<br>
```
java.lang.IllegalStateException: closed
```
{% highlight kotlin linenos %}
val bodyString = body?.string()
{% endhighlight %}

{% highlight kotlin linenos %}
suspend fun async_get() = withContext(Dispatchers.IO) {
  val request = Request.Builder()
    .url("https://www.httpbin.org/get")
    .get()
    .build()

  client.newCall(request).execute().use { response ->
    if (response.isSuccessful) {
      response.body().let { body ->
        val bodyString = body?.string()
        println("body = $bodyString")
        bodyString
      }
    } else {
      null
    }
  }
}

@Test
fun test1() = runBlocking {
  async_get()
  println()
}
{% endhighlight %}


[1]: {% link _pages/kotlin/withcontext.md %}