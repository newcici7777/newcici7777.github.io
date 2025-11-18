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

## Interface 產生介面的實作物件
Interface
{% highlight kotlin linenos %}
interface ApiService {
  @GET("get")
  suspend fun get(@Query("name") name: String) : Response<ResponseBody>
}
{% endhighlight %}

產生介面的實作物件
{% highlight kotlin linenos %}
val service = retrofit.create(ApiService::class.java)
{% endhighlight %}

ApiService::class 是 Kotlin 的 KClass 類別 <br>
ApiService::class.java 把 Kotlin 的 KClass 轉成 Java 的 Class <br>

Retrofit 是一個 Java 寫的函式庫，它的 create() 方法長這樣：
```
public <T> T create(Class<T> service);
```

所以它要的參數是 Java Class<T>，在 Kotlin 世界裡，就要加上 .java 才能轉換成Java class。

|Kotlin 寫法 |Java 寫法| 說明|
|:-------|:------|:-----------|
|ApiService::class |ApiService.class  |Kotlin 專用的 KClass|
|ApiService::class.java|  ApiService.class|  Retrofit 要的 Java Class|

Retrofit 執行這行時，實際做了這件事：

幫你「生成一個實作了 ApiService 的代理類」，
這個代理類會攔截你呼叫的 getData() 方法，
然後組成一個 HTTP 請求發出去。

如果你有多個 API 介面，也可以都用這樣創建：

```
val userApi = retrofit.create(UserApi::class.java)
val orderApi = retrofit.create(OrderApi::class.java)
```

每個 create() 都會生成一個獨立的代理實例。

ApiService::class.java 是在告訴 Retrofit：「幫我生成這個介面的實作物件」，<br>
Retrofit 會在執行時根據介面的標註（@GET、@POST...）自動幫你組成 HTTP 請求。

## Annotations
### @GET
Api
{% highlight kotlin linenos %}
interface ApiService {
  @GET("get")
  suspend fun get(@Query("name") name: String) : Response<ResponseBody>
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
}
{% endhighlight %}
```
{
  "args": {
    "name": "Alice"
  }, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-691133af-3ccdfc272a50f4927d7d3aa2"
  }, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/get?name=Alice"
}
```
### @Post
#### @Post @FormUrlEncoded
@FormUrlEncoded 是用來告訴 Retrofit：

這是一個 表單格式 (application/x-www-form-urlencoded) 的 POST 請求。

也就是說，它是用來搭配 @Field 一起使用的。
它會自動幫你把欄位轉成像這樣的 body：

user=alice&password=1234

與 @Body 的差別

|  標註|  傳輸格式|  用於|
|:-----------|:-----------|:-------|
|@FormUrlEncoded + @Field|  application/x-www-form-urlencoded| 傳送普通表單欄位（文字）  |
|@Body |取決於你傳的 RequestBody  |傳送 JSON、FormBody、Multipart... 等任何格式| 

Retrofit 幫你送出的 HTTP 請求會是這樣：

```
POST /post HTTP/1.1
Content-Type: application/x-www-form-urlencoded
```

注意：必須同時加上 @FormUrlEncoded

如果你寫了 @Field 卻沒加 @FormUrlEncoded，
Retrofit 會報錯：
```
@Field parameters can only be used with form encoding. (parameter #1)
````
{% highlight kotlin linenos %}
interface ApiService {
  @POST("post")
  @FormUrlEncoded
  suspend fun post(@Field("name") name: String) : Response<ResponseBody>
}
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun testPost() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.post("Alice")
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
    "name": "Alice"
  }, 
  "headers": {
    "Content-Length": "10", 
    "Content-Type": "application/x-www-form-urlencoded", 
  }, 
  "json": null, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```
#### @Post @Body FormBody
@Body 傳 RequestBody 任意格式（JSON、FormBody）
```
POST /post HTTP/1.1
Content-Type: application/x-www-form-urlencoded
```

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
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "24", 
    "Content-Type": "application/x-www-form-urlencoded", 
  }, 
  "json": null, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```
#### @Post @Body json
```
POST /post HTTP/1.1
Content-Type: application/json
```
{% highlight kotlin linenos %}
  @POST("post")
  suspend fun postjson(@Body body: RequestBody) : Response<ResponseBody>
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun json() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val json = """
      {
      "a": 1,
      "b": 2
      }
    """.trimIndent()
    val jsonbody = RequestBody.create(MediaType.parse("application/json"), json)
    val response = service.postjson(jsonbody)
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
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
    "X-Amzn-Trace-Id": "Root=1-6911376b-1f3523996a6ee09c63a456e8"
  }, 
  "json": {
    "a": 1, 
    "b": 2
  }, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```
#### @Post @Body 物件轉json
build.gradle(:app)
```
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
```

加入Gson轉換工廠，可以把物件轉成json
```
addConverterFactory(GsonConverterFactory.create())
```
{% highlight kotlin linenos %}
val retrofitGson = Retrofit.Builder()
  .baseUrl("https://www.httpbin.org/")
  .addConverterFactory(GsonConverterFactory.create())
  .build()
{% endhighlight %}

物件
{% highlight kotlin linenos %}
data class UserInfo(
  val name: String, val password: String
)
{% endhighlight %}

Api Interface
{% highlight kotlin linenos %}
  @POST("post")
  suspend fun postObj(@Body body: UserInfo): Response<ResponseBody>
{% endhighlight %}

執行結果Content-Type是application/json，有json欄位。
{% highlight kotlin linenos %}
  @Test
  fun postObj() = runBlocking<Unit> {
    val retrofitGson = Retrofit.Builder()
      .baseUrl("https://www.httpbin.org/")
      .addConverterFactory(GsonConverterFactory.create())
      .build()
    val service = retrofitGson.create(ApiService::class.java)
    val userInfo = UserInfo("Mary", "1234")
    val response = service.postObj(userInfo)
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
  "args": {}, 
  "data": "{\"name\":\"Mary\",\"password\":\"1234\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "33", 
    "Content-Type": "application/json; charset=UTF-8", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-69114062-43c94ace06011c254a13d201"
  }, 
  "json": {
    "name": "Mary", 
    "password": "1234"
  }, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```
#### @Post @Multipart 上傳檔案
`@Part`要有欄位名。
```
@Part("欄位名")
@Part("desc")
```
{% highlight kotlin linenos %}
  @Multipart
  @POST("post")
  suspend fun postMultipart(
    @Part file: okhttp3.MultipartBody.Part, 
    @Part("desc") body: RequestBody
  ): Response<ResponseBody>
{% endhighlight %}


{% highlight kotlin linenos %}
  @Test
  fun postMultipart() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val file = File("/Users/cici/Desktop/data")
    val filebody = RequestBody.create(MediaType.parse("text/plain"), file)
    // 傳送檔案
    val part1 = MultipartBody.Part.createFormData("file1", file.name, filebody)
    // 傳送文字
    val text = RequestBody.create(MediaType.parse("text/plain"), "text content")
    val response = service.postMultipart(part1, text)
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
  "args": {}, 
  "data": "", 
  "files": {
    "file1": "test"
  }, 
  "form": {
    "desc": "text content"
  }, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "395", 
    "Content-Type": "multipart/form-data; boundary=8d40d716-b986-40ff-972c-dd585d7be6ae", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-69113da4-02eb34df1288b0754d9ee2e6"
  }, 
  "json": null, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```
### @Http
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
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-691134f5-273506e65ab818ff6fd29a98"
  }, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/get?name=Alice"
}
```

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
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "0", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-691134b8-3005b6a511d732a3647311dd"
  }, 
  "json": null, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post?name=Alice"
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
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-6911353c-7c4f426a393b6e1933b42f71"
  }, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/get?name=Alice&address=Taiwan"
}
```

### @Path
@Path通常搭配頁碼。
```
https://你的baseUrl/page/{pageNumber}
https://你的baseUrl/page/1
```

由於我是使用使用httpbin，目前沒看到有page相關API，使用action參數代表要做的動作是post。
{% highlight kotlin linenos %}
  @POST("{action}")
  @FormUrlEncoded
  suspend fun pathTest(
    @Path("action") action: String,
    @Field("name") name: String
  ): Response<ResponseBody>
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun path() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.pathTest("post", "Mary")
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
    "name": "Mary"
  }, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "9", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-6911493c-20910e7754dfc70b1b5857b6"
  }, 
  "json": null, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```

### @Header
Api
{% highlight kotlin linenos %}
  @POST("post")
  suspend fun postHeader(@Body body: RequestBody, @Header("version") version: String): Response<ResponseBody>
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun testHeader() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val formbody = FormBody.Builder()
      .add("user", "alice")
      .add("password", "1234")
      .build()
    val response = service.postHeader(formbody, "version1.0")
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
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "24", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "Version": "version1.0", 
    "X-Amzn-Trace-Id": "Root=1-69116368-4a2f0b0b79c617d04044d7c3"
  }, 
  "json": null, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```
### @Headers
注意！後面有`s`

Api
{% highlight kotlin linenos %}
  @Headers("os:android","version:1.0")
  @POST("post")
  suspend fun postHeaderS(@Body body: RequestBody): Response<ResponseBody>

{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun testHeaderS() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val formbody = FormBody.Builder()
      .add("user", "alice")
      .add("password", "1234")
      .build()
    val response = service.postHeaderS(formbody)
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
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "24", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "www.httpbin.org", 
    "Os": "android", 
    "User-Agent": "okhttp/3.14.9", 
    "Version": "1.0", 
    "X-Amzn-Trace-Id": "Root=1-69116545-7fac0a3e68612ea7151b8451"
  }, 
  "json": null, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```
### @Url
自己定義url，會覆蓋baseURL。<br>

注意！下方的@Get後面是沒有東西，沒有圓括號()。
{% highlight kotlin linenos %}
  @GET
  suspend fun getUrl(@Url url: String): Response<ResponseBody>
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun testUrl() = runBlocking<Unit> {
    val service = retrofit.create(ApiService::class.java)
    val response = service.getUrl("https://www.httpbin.org/get?name=Mary")
    if (response.isSuccessful) {
      println(response.body()?.string())
    }
  }
{% endhighlight %}
```
{
  "args": {
    "name": "Mary"
  }, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-6911872f-7c5e2e00125e9a7574732956"
  }, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/get?name=Mary"
}
```
## 回傳物件
以下是回傳的Response，要把json欄位的內容轉成UserInfo的物件回傳。
```
{
  "args": {}, 
  "data": "{\"name\":\"Mary\",\"password\":\"1234\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept-Encoding": "gzip", 
    "Content-Length": "33", 
    "Content-Type": "application/json; charset=UTF-8", 
    "Host": "www.httpbin.org", 
    "User-Agent": "okhttp/3.14.9", 
    "X-Amzn-Trace-Id": "Root=1-69115c37-615e97ce087bac434ab53385"
  }, 
  "json": {
    "name": "Mary", 
    "password": "1234"
  }, 
  "origin": "42.73.166.13", 
  "url": "https://www.httpbin.org/post"
}
```

### Response`<物件>`
尖括號包著的是回傳的物件，使用Response作為回值類型，是可以使用isSuccessful屬性，檢查回傳值是200。<br>
```
if (response.isSuccessful) { ... }
```
### GsonConvert
build.gradle(:app)
```
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
```

寫一個ResponseWrapper1，欄位名json一定要對映Response中的json欄位名，json的類型為 UserInfo 
{% highlight kotlin linenos %}
data class ResponseWrapper1(
  val json: UserInfo
)
{% endhighlight %}

UserInfo類別
{% highlight kotlin linenos %}
data class UserInfo(
  val name: String, val password: String
)
{% endhighlight %}

Api
{% highlight kotlin linenos %}
  @POST("post")
  suspend fun rtnObj1(@Body body: UserInfo): Response<ResponseWrapper1>
{% endhighlight %}

{% highlight kotlin linenos %}
  @Test
  fun postObjRtn1() = runBlocking<Unit> {
    val retrofitGson = Retrofit.Builder()
      .baseUrl("https://www.httpbin.org/")
      .addConverterFactory(GsonConverterFactory.create())
      .build()
    val service = retrofitGson.create(ApiService::class.java)
    val userInfo = UserInfo("Mary", "1234")
    val response = service.rtnObj1(userInfo)
    if (response.isSuccessful) {
      val wrapper = response.body()
      val usr = wrapper?.json
      println("name = ${usr?.name} password = ${usr?.password}")
    }
  }
{% endhighlight %}
```
name = Mary password = 1234
```

#### @SerializedName
前一個範例遇到的問題，只能使用json欄位名，使用@SerializedName處理這個問題。<br>
告訴gson，把json欄位，轉成userInfo的物件。<br>
{% highlight kotlin linenos %}
import com.google.gson.annotations.SerializedName

data class ResponseWrapper1(
  @SerializedName("json")
  val userInfo: UserInfo
)
{% endhighlight %}

其它內容都一樣，只是`wrapper?.userInfo`有改變。<br>
{% highlight kotlin linenos %}
  @Test
  fun postObjRtn1() = runBlocking<Unit> {
    val retrofitGson = Retrofit.Builder()
      .baseUrl("https://www.httpbin.org/")
      .addConverterFactory(GsonConverterFactory.create())
      .build()
    val service = retrofitGson.create(ApiService::class.java)
    val userInfo = UserInfo("Mary", "1234")
    val response = service.rtnObj1(userInfo)
    if (response.isSuccessful) {
      val wrapper = response.body()
      val usr = wrapper?.userInfo
      println("name = ${usr?.name} password = ${usr?.password}")
    }
  }
{% endhighlight %}
```
name = Mary password = 1234
```

### moshi
除了使用@SerializedName("json")，也可以使用moshi處理這個問題。<br>

build.gradle(:app)
```
    implementation "com.squareup.retrofit2:converter-moshi:2.9.0"
    def moshi_version = "1.13.0"
    implementation "com.squareup.moshi:moshi-kotlin:$moshi_version"
```

Response的欄位為json，把它轉成userInfo的欄位名，類型為UserInfo。<br>
{% highlight kotlin linenos %}
import com.squareup.moshi.Json
import com.squareup.moshi.JsonClass
@JsonClass(generateAdapter = true)
data class ResponseWrapper2(
  @Json(name = "json")
  val userInfo: UserInfo
)
{% endhighlight %}

Api
{% highlight kotlin linenos %}
  @POST("post")
  suspend fun rtnObj2(@Body body: UserInfo): Response<ResponseWrapper2>
{% endhighlight %}

使用Moshi轉換工廠。<br>
{% highlight kotlin linenos %}
  @Test
  fun postObjRt2() = runBlocking<Unit> {
    val retrofitGson = Retrofit.Builder()
      .baseUrl("https://www.httpbin.org/")
      .addConverterFactory(
        //建立Moshi工廠
        MoshiConverterFactory.create(
          Moshi.Builder()
            .add(KotlinJsonAdapterFactory())
            .build()
        )
      )
      .build()
    val service = retrofitGson.create(ApiService::class.java)
    val userInfo = UserInfo("Mary", "1234")
    val response = service.rtnObj2(userInfo)
    if (response.isSuccessful) {
      val wrapper = response.body()
      val usr = wrapper?.userInfo
      println("name = ${usr?.name} password = ${usr?.password}")
    }
  }
{% endhighlight %}
```
name = Mary password = 1234
```

原理:<br>
MoshiConverterFactory 是什麼？

MoshiConverterFactory 是 Retrofit 的 Converter Factory，用來把 HTTP 回傳的 JSON 自動轉成 Kotlin/Java 物件，或把 Kotlin/Java 物件轉成 JSON 傳給伺服器。

它背後使用 Moshi 這個 JSON 解析庫（由 Square 團隊開發，和 Retrofit 同家族）。

作用就像你之前用的 GsonConverterFactory，只是底層是 Moshi 而不是 Gson。

Moshi 原生支援 Kotlin 的 data class

你用了：
```
@JsonClass(generateAdapter = true)
```

這會自動生成 Adapter，幫 Moshi 處理 Kotlin 的 constructor、nullable、default value 等特性。

Gson 有時在處理 nested JSON 或 字串包 JSON 的情況下會比較麻煩，需要手動 type adapter 或額外解析。

@Json(name="json")

這行會告訴 Moshi，把 JSON 回傳裡的 "json" 欄位對應到 userInfo 屬性。

Moshi 會自動解析 "json" 內的物件成 UserInfo，你不需要手動呼叫 Gson().fromJson()。

你呼叫：
```
val response = service.rtnObj(userInfo)
```

Retrofit 發出 POST 請求，把 UserInfo 轉成 JSON（Moshi 轉換）。

伺服器回傳：
```
{
  "args": {},
  "data": "{\"name\":\"Mary\",\"password\":\"1234\"}",
  "json": {
      "name": "Mary",
      "password": "1234"
  }
}
```

MoshiConverterFactory 看到你定義：
```
@JsonClass(generateAdapter = true)
data class ResponseWrapper(
  @Json(name = "json") val userInfo: UserInfo
)
```
就自動把 json 裡的物件解析成 UserInfo，然後包在 ResponseWrapper 裡回傳給你。

## RetrofitUtil
以下內容知識量很多，必須要擁有以下知識，才可以繼續。<br>

Prerequisites:

- [Class][1]
- [反射][2]
- [二個冒號::引用][3]
- [泛型][4]
- [object_companion][5]
- [by lazy][6]
- [by][7]

參數是Java Class(Java類別描述檔)，因為Retrofit是java寫的，所以create()方法只能用java的class傳入。<br>

使用泛型方法定義的參數T。<br>
{% highlight kotlin linenos %}
  fun <T> createService(clazz: Class<T>):T {
    return retrofit.create(clazz)
  }
{% endhighlight %}

retrofit是靜態變數，使用by委托給lazy，只執行一次`by lazy{}`的內容，之後就不會再執行，用到retrofit的屬性的時候才會執行。<br>

`ArticleApi::class.java`，把KClass轉成Java Class，把Java Class作為參數傳給方法。<br>
{% highlight kotlin linenos %}
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

完整程式碼
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

### Data class
data class自動有setter與getter功能，不用自己寫。
{% highlight kotlin linenos %}
data class ArticleList(val data: List<ArticleItem>, val code: Int = -1, val message: String)
{% endhighlight %}

{% highlight kotlin linenos %}
data class ArticleItem(val title: String, val source: String, val time: String)
{% endhighlight %}

### ServiceApi
articleList()傳回值是ArticleList，ArticleList又包含ArticleItem。

注意以下article前面無右斜線。<br>
```
@GET("article/list")
```
{% highlight kotlin linenos %}
package com.example.coroutine.api
import retrofit2.http.GET

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

### 使用ArticleApi.retrofit
呼叫ArticleApi.retrofit.articleList()，就可以執行retrofit的網路請求。<br>
retrofit會把interface轉成物件。<br>
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

[1]: {% link _pages/kotlin/java_class.md %}
[2]: {% link _pages/kotlin/kotlin_reflect.md %}
[3]: {% link _pages/kotlin/refer_operator.md %}
[4]: {% link _pages/kotlin/generics.md %}
[5]: {% link _pages/kotlin/object_companion.md %}
[6]: {% link _pages/kotlin/lazy.md %}
[7]: {% link _pages/kotlin/delegate.md %}
