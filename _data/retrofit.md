---
title: retrofit
date: 2025-10-14
keywords: kotlin, retrofit
---
app下的build.gradle
{% highlight groovy linenos %}
// Retrofit
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
{% endhighlight %}


RetrofitUtil(class)
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