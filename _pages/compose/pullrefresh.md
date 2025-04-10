---
title: PullRefresh
date: 2023-05-03
keywords: Android, Jetpack compose, PullRefresh
---
在ArticleViewModel加上
{% highlight kotlin linenos %}
var refreshing by mutableStateOf(false)
  private set suspend fun refresh() {
    pageOffset = 1
    refreshing = true
    fetchArticleList()
}
{% endhighlight %}

修改既有方法
{% highlight kotlin linenos %}
suspend fun fetchArticleList(){
   val res = articleService.list(pageOffset = pageOffset, pageSize = pageSize)
    delay(3000)
    if(res.code == 0 && res.data != null) {
        list = res.data
        listLoaded = true //是否載入完畢
        refreshing = false
    }
}
{% endhighlight %}

在StudyScreen先import
{% highlight kotlin linenos %}
import androidx.compose.material.pullrefresh.PullRefreshIndicator
import androidx.compose.material.pullrefresh.pullRefresh
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.rememberCoroutineScope
import androidx.compose.runtime.setValue
import kotlinx.coroutines.launch
{% endhighlight %}

{% highlight kotlin linenos %}
val lazyListState = rememberLazyListState()
val coroutineScope = rememberCoroutineScope()
var refreshing by remember { mutableStateOf(articleViewModel.refreshing) }
val pullRefreshState = rememberPullRefreshState(refreshing, { coroutineScope.launch { articleViewModel.refresh() } })
Box(Modifier.pullRefresh(pullRefreshState)) {
    LazyColumn(state = lazyListState) {
    }
    PullRefreshIndicator(refreshing, pullRefreshState, Modifier.align(Alignment.TopCenter))
}
{% endhighlight %}