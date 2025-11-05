---
title: Okhttp
date: 2025-10-23
keywords: kotlin, Okhttp
---
Prerequisites:

- [withContext][1]

## client
{% highlight kotlin linenos %}
val client = OkHttpClient()
{% endhighlight %}

## 同步 非同步 get post

|類型|	特點|	使用方法|
|:------|:------------|:--------|
|同步 (Synchronous)|	會阻塞目前執行緒，直到伺服器回應|	call.execute()|
|非同步 (Asynchronous)|	不阻塞執行緒，回應會在callback 回呼中收到	|call.enqueue(callback)|

所謂的阻塞(Blocking)，就是要等待網路連線回應，才進行下一個程式碼執行。<br>

非同步，就是不用等待網路連線，可以先執行其它程式碼，不用卡在那邊等待網路回應。<br>

response.isSuccessful Code 介於200 - 299

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
```
body = {
  "args": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-690464d0-37ae6a0d1b229d9021792d47"
  }, 
  "origin": "42.72.144.14", 
  "url": "https://www.httpbin.org/get"
}
```

## post formbody
{% highlight kotlin linenos %}
  suspend fun post_formbody() = withContext(Dispatchers.IO) {
    val formbody = FormBody.Builder()
      .add("user", "alice")
      .add("password","1234")
      .build()
    val request = Request.Builder()
      .url("https://www.httpbin.org/post")
      .post(formbody)
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
  fun test2() = runBlocking {
    post_formbody()
    println()
  }
{% endhighlight %}
```
body = {
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "password": "1234", 
    "user": "alice"
  }, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "24", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-69046a91-76b1eb606c4fa64b450a6946"
  }, 
  "json": null, 
  "origin": "42.72.144.14", 
  "url": "https://www.httpbin.org/post"
}
```

## post MultiBody
{% highlight kotlin linenos %}
  suspend fun post_multibody() = withContext(Dispatchers.IO) {
    val file = File("/Users/cici/Desktop/data")
    val file2 = File("/Users/cici/Desktop/data")
    val multibody = MultipartBody.Builder()
      .addFormDataPart("file1",file.name, RequestBody.create(MediaType.parse("text/plain"),file))
      .addFormDataPart("file2",file2.name, RequestBody.create(MediaType.parse("text/plain"),file2))
      .addFormDataPart("key","word")
      .build()
    val request = Request.Builder()
      .url("https://www.httpbin.org/post")
      .post(multibody)
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
  fun test2() = runBlocking {
    post_multibody()
    println()
  }
{% endhighlight %}
```
body = {
  "args": {}, 
  "data": "--0b9d51e2-59a7-4b5c-b627-e19af6c7aba3\r\nContent-Disposition: form-data; name=\"file1\"; filename=\"data\"\r\nContent-Type: text/plain\r\nContent-Length: 4\r\n\r\ntest\r\n--0b9d51e2-59a7-4b5c-b627-e19af6c7aba3\r\nContent-Disposition: form-data; name=\"file2\"; filename=\"data\"\r\nContent-Type: text/plain\r\nContent-Length: 4\r\n\r\ntest\r\n--0b9d51e2-59a7-4b5c-b627-e19af6c7aba3\r\nContent-Disposition: form-data; name=\"key\"\r\nContent-Length: 4\r\n\r\nword\r\n--0b9d51e2-59a7-4b5c-b627-e19af6c7aba3--\r\n", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "465", 
    "Content-Type": "multipart/mixed; boundary=0b9d51e2-59a7-4b5c-b627-e19af6c7aba3", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-690ab00e-62dca96626ffe52f55c2252f"
  }, 
  "json": null, 
  "origin": "42.72.22.52", 
  "url": "https://www.httpbin.org/post"
}
```
## post json
{% highlight kotlin linenos %}
  suspend fun post_json() = withContext(Dispatchers.IO) {
    val file = File("/Users/cici/Desktop/data")
    val file2 = File("/Users/cici/Desktop/data")
    val json = """
      {
      "a": 1,
      "b": 2
      }
    """.trimIndent()
    val jsonbody = RequestBody.create(MediaType.parse("application/json"),json)
    val request = Request.Builder()
      .url("https://www.httpbin.org/post")
      .post(jsonbody)
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
  fun test2() = runBlocking {
    post_json()
    println()
  }
{% endhighlight %}
```
body = {
  "args": {}, 
  "data": "{\n\"a\": 1,\n\"b\": 2\n}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "18", 
    "Content-Type": "application/json; charset=utf-8", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-690ab308-65c55b372f9532b66cbfc63b"
  }, 
  "json": {
    "a": 1, 
    "b": 2
  }, 
  "origin": "42.72.22.52", 
  "url": "https://www.httpbin.org/post"
}
```
## add header
{% highlight kotlin linenos %}
  suspend fun addHead() = withContext(Dispatchers.IO) {
    val request = Request.Builder()
      .url("https://www.httpbin.org/get")
      .addHeader("version", "1.0")
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
  fun test2() = runBlocking {
    addHead()
    println()
  }
{% endhighlight %}
```
body = {
  "args": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "Version": "1.0", 
    "X-Amzn-Trace-Id": "Root=1-690ab9ba-51c09892027e64587ee81543"
  }, 
  "origin": "42.72.22.52", 
  "url": "https://www.httpbin.org/get"
}
```

## Interceptor NetworkInterceptor
{% highlight kotlin linenos %}
  suspend fun interceptor() = withContext(Dispatchers.IO) {
    val client = OkHttpClient.Builder()
      .addInterceptor { chain ->
        val newRequest = chain.request().newBuilder().addHeader("os", "android").build()
        val response = chain.proceed(newRequest)
        response
      }
      .addNetworkInterceptor { chain ->
        val request = chain.request()
        println("request = ${request.url()}")
        val response = chain.proceed(request)
        println("response = ${response.headers()}")
        response
      }
      .build()
    val request = Request.Builder()
      .url("https://www.httpbin.org/get")
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
  fun test2() = runBlocking {
    interceptor()
    println()
  }
{% endhighlight %}
```
request = https://www.httpbin.org/get
response = date: Wed, 05 Nov 2025 02:40:11 GMT
content-type: application/json
content-length: 295
server: gunicorn/19.9.0
access-control-allow-origin: *
access-control-allow-credentials: true

body = {
  "args": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "www.httpbin.org", 
    "Os": "android", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-690ab90a-6d7f9b181b85a79446419212"
  }, 
  "origin": "42.72.22.52", 
  "url": "https://www.httpbin.org/get"
}
```
## cookie
登入後，再使用其它網址，不會再請user登入。
### 簡單測試
{% highlight kotlin linenos %}
  suspend fun cookie() = withContext(Dispatchers.IO) {
    val cookieStore = mutableMapOf<String,List<Cookie>>()
    val client1 = OkHttpClient.Builder()
      .cookieJar(object : CookieJar {
        override fun saveFromResponse(
          url: HttpUrl,
          cookies: List<Cookie>
        ) {
          cookieStore.put(url.host(),cookies)
        }

        override fun loadForRequest(url: HttpUrl): List<Cookie> {
          val cookie = cookieStore[url.host()] ?: emptyList()
          return cookie
        }
      })
      .build()

    val request = Request.Builder()
      .url("https://www.httpbin.org/cookies/set?name=cici")
      .build()

    client1.newCall(request).execute().use { response ->
      println("response = ${response.code()}")
    }

    val request2 = Request.Builder()
      .url("https://www.httpbin.org/cookies")
      .build()
    client1.newCall(request2).execute().use { response ->
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
  fun test2() = runBlocking {
    cookie()
    println()
  }
{% endhighlight %}
```
response = 200
body = {
  "cookies": {
    "name": "cici"
  }
}
```
### 複雜測試
先到下方網站註冊

<https://wanandroid.com/blog/show/2>

使用右邊列表，5. 登录与注册的API
```
5. 登录与注册
5.1 登录
```
{% highlight kotlin linenos %}
  suspend fun login() = withContext(Dispatchers.IO) {
    val formbody = FormBody.Builder()
      .add("username", "xxx")
      .add("password", "xxx")
      .build()
    val request = Request.Builder()
      .url("https://www.wanandroid.com/user/login")
      .post(formbody)
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
  fun test2() = runBlocking {
    login()
    println()
  }
{% endhighlight %}

重點:
1. 要建立新的OkHttpClient.Builder().cookieJar()，不能再既有的client使用.cookieJar()
2. List<Cookie> 不能為null，不能是 List<Cookie?>


儲存cookie，先登入，收藏文章後，再執行。
{% highlight kotlin linenos %}

  suspend fun login() = withContext(Dispatchers.IO) {
    val cookieStore = mutableMapOf<String,List<Cookie>>()
    val client1 = OkHttpClient.Builder()
      .cookieJar(object : CookieJar {
        override fun saveFromResponse(
          url: HttpUrl,
          cookies: List<Cookie>
        ) {
          cookieStore.put(url.host(),cookies)
        }

        override fun loadForRequest(url: HttpUrl): List<Cookie> {
          val cookie = cookieStore[url.host()] ?: emptyList()
          return cookie
        }
      })
      .build()
    val formbody = FormBody.Builder()
      .add("username", "xxx")
      .add("password", "xxx")
      .build()

    // 第一次登入
    val request = Request.Builder()
      .url("https://www.wanandroid.com/user/login")
      .post(formbody)
      .build()

    client1.newCall(request).execute().use { response ->
      println("response = ${response.code()}")
    }

    // 登入後，看收藏文章，已經不用代入帳號密碼
    val request2 = Request.Builder()
      .url("https://www.wanandroid.com/lg/collect/list/0/json")
      .build()
    client1.newCall(request2).execute().use { response ->
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
  fun test2() = runBlocking {
    login()
    println()
  }
{% endhighlight %}


[1]: {% link _pages/kotlin/withcontext.md %}