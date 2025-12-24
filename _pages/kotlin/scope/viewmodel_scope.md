---
title: viewModelScope
date: 2025-10-14
keywords: Android, Kotlin, coroutines, viewModelScope
---
Prerequisites:

- [retrofit][1]
- [stateflow][2]
- [retrofit stateflow][3]

## 使用時機
ViewModel 中的資料操作:
- API 呼叫
- 資料庫操作
- 商業邏輯

 當 ViewModel 結束時自動清除協程，不會記憶體洩露。

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

{% highlight kotlin linenos %}
class ArticleViewModel : ViewModel() {
  val _articleList = MutableStateFlow<ArticleList?>(null)
  val articleList: StateFlow<ArticleList?> = _articleList
  fun getArticleList() {
    viewModelScope.launch {
      val result = ArticleApi.retrofit.articleList()
      _articleList.value = result
    }
  }
}
{% endhighlight %}


[1]: {% link _pages/kotlin/retrofit.md %}
[2]: {% link _pages/kotlin/stateflow.md %}
[3]: {% link _pages/kotlin/retrofit_stateflow.md %}
