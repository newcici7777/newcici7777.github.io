---
title: retrofit
date: 2025-10-14
keywords: kotlin, retrofit
---
## app下的build.gradle
{% highlight groovy linenos %}
// Retrofit
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
{% endhighlight %}

## 建立Retrofit
{% highlight kotlin linenos %}
  private val retrofit = Retrofit.Builder()
    .baseUrl("https://www.httpbin.org/")
    .build()
{% endhighlight %}

## interface
可以使用interface的方法，呼叫api。
{% highlight kotlin linenos %}
val service = retrofit.create(ApiService::class.java)
{% endhighlight %}

## Annotations
### @GET @POST
Api
{% highlight kotlin linenos %}
interface ApiService {
  @GET("get")
  suspend fun get(@Query("name") name: String) : Response<ResponseBody>
  @POST("post")
  @FormUrlEncoded
  suspend fun post(@Field("name") name: String) : Response<ResponseBody>
}
{% endhighlight %}

TestCase
{% highlight kotlin linenos %}
import kotlinx.coroutines.runBlocking
import org.junit.Test
import retrofit2.Retrofit

class RetrofitTest {
  private val retrofit = Retrofit.Builder()
    .baseUrl("https://www.httpbin.org/")
    .build()

  @Test
  fun testGet() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.get("Alice")
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
  @Test
  fun testPost() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.post("Alice")
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
}
{% endhighlight %}

### @Http
#### @Http post
自訂method，hasBody是指「是否傳送Body」，path是路徑。
{% highlight kotlin linenos %}
  @HTTP(method = "POST", path = "post", hasBody = true)
  suspend fun httpPost(@Query("name") name: String) : Response<ResponseBody>
{% endhighlight %}

hasBody為true，執行結果有json欄位。
{% highlight kotlin linenos %}
  @Test
  fun httpPost() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.httpPost("Alice")
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
  "args": {
    "name": "Alice"
  }, 
  "data": "", 
  "files": {}, 
  "form": {}, 
  "json": null, 
  "url": "https://www.httpbin.org/post?name=Alice"
}
```
#### @Http get
自訂method，hasBody是指「是否傳送Body」，path是路徑。
{% highlight kotlin linenos %}
  @HTTP(method = "GET", path = "get", hasBody = false)
  suspend fun httpGet(@Query("name") name: String) : Response<ResponseBody>
{% endhighlight %}

hasBody為false，執行結果沒有json欄位。
{% highlight kotlin linenos %}
  @Test
  fun httpGet() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.httpGet("Alice")
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
  "args": {
    "name": "Alice"
  }, 
  "url": "https://www.httpbin.org/get?name=Alice"
}
```

### @QueryMap
可以把map作為參數。
{% highlight kotlin linenos %}
  @GET("get")
  suspend fun queryMap(@QueryMap map: Map<String, String>) : Response<ResponseBody>
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun queryMap() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.queryMap(
      mapOf(
        "name" to "Alice",
        "address" to "Taiwan"
      )
    )
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
  "args": {
    "address": "Taiwan", 
    "name": "Alice"
  }, 
  "url": "https://www.httpbin.org/get?name=Alice&address=Taiwan"
}
```

### @Body FormBody
類似okhttp的FormBody，以下是okHttp的FormBody。
{% highlight kotlin linenos %}
    val formbody = FormBody.Builder()
      .add("user", "alice")
      .add("password","1234")
      .build()
{% endhighlight %}

ApiService
{% highlight kotlin linenos %}
  @POST("post")
  suspend fun postbody(@Body body: RequestBody) : Response<ResponseBody>
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun formBody() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val formbody = FormBody.Builder()
      .add("user", "alice")
      .add("password","1234")
      .build()

    val response = service.postbody(formbody)
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "password": "1234", 
    "user": "alice"
  }, 
  "json": null, 
  "url": "https://www.httpbin.org/post"
}
```

## RetrofitUtil(class)
{% highlight kotlin linenos %}
package com.example.coroutine.api

import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object RetrofitUtil {
  private const val BASE_URL =
    "https://mock.apipost.cn/app/mock/project/ced69cf2-9206-4a42-895e-dd7442a888df/"
  private val retrofit = Retrofit.Builder()
    .addConverterFactory(GsonConverterFactory.create())
    .baseUrl(BASE_URL)
    .build()

  fun <T> createService(clazz: Class<T>):T {
    return retrofit.create(clazz)
  }
}
{% endhighlight %}

ArticleApi.kt包含Item與List的data class(data class自動有setter與getter功能)。<br>
interface ArticleApi 包含companion object，retrofit變數會初始化網路傳輸的api。<br>
注意以下article前面無右斜線。<br>
```
@GET("article/list")
```


ArticleApi.kt
{% highlight kotlin linenos %}
package com.example.coroutine.api

import retrofit2.http.GET

data class ArticleItem(val title: String, val source: String, val time: String)

data class ArticleList(val data: List<ArticleItem>, val code: Int = -1, val message: String)

interface ArticleApi {
  @GET("article/list")
  suspend fun articleList(): ArticleList
  companion object {
    val retrofit: ArticleApi by lazy {
      RetrofitUtil.createService(ArticleApi::class.java)
    }
  }
}
{% endhighlight %}

呼叫ArticleApi.retrofit.articleList()
{% highlight kotlin linenos %}
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext

class MainActivity07 : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)
    submit.setOnClickListener {
      GlobalScope.launch(Dispatchers.Main) {
        val listResult = withContext(Dispatchers.IO) {
          ArticleApi.retrofit.articleList()
        }
        textv.text = listResult.toString()
      }
    }
  }
}
{% endhighlight %}