---
title: Okhttp
date: 2025-10-23
keywords: kotlin, Okhttp
---
## 同步 非同步 get post
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