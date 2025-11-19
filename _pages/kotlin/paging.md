---
title: paging 3
date: 2025-11-19
keywords: kotlin, paging 
---
## api
pageOffset是頁碼，pageSize是每頁大小。<br>
{% highlight kotlin linenos %}
import com.example.coroutine.repository.ArticleList
import retrofit2.http.GET
import retrofit2.http.Query

interface ArticleApi {
  @GET("article/list")
  suspend fun list(@Query("pageOffset") pageOffset: Int,
                   @Query("pageSize") pageSize: Int): ArticleList
  companion object {
    val retrofit: ArticleApi by lazy {
      RetrofitUtil.createService(ArticleApi::class.java)
    }
  }
}
{% endhighlight %}

## PagingSource
第1個泛型參數Int是固定。<br>
第2個泛型參數為回傳的物件的類別。<br>
```
PagingSource<Int, ArticleItem>()
```

- prevKey 是上一頁，若是第1頁，它的上一頁是 null(沒有上一頁)。<br>
- nextKey 是下一頁，若資料為空，就傳null(沒有下一頁)。<br>

{% highlight kotlin linenos %}
import androidx.paging.PagingSource
import androidx.paging.PagingState
import com.example.coroutine.api.ArticleApi
import com.example.coroutine.repository.ArticleItem

class ArticlePagingSource : PagingSource<Int, ArticleItem>() {
  override fun getRefreshKey(state: PagingState<Int, ArticleItem>): Int? {
    TODO("Not yet implemented")
  }
  override suspend fun load(params: LoadParams<Int>): LoadResult<Int, ArticleItem> {
  	// 若params.key 為空，設為第1頁
    val page = params.key ?: 1
    val pageSize = params.loadSize
    return try {
      val response = ArticleApi.retrofit.list(page, pageSize)
      LoadResult.Page(
        data = response.data,
        prevKey = if (page == 1) null else page - 1,
        nextKey = if (response.data.isEmpty()) null else page + 1
      )
    } catch (e: Exception) {
      LoadResult.Error(e)
    }
  }
}
{% endhighlight %}

## ViewModel
{% highlight kotlin linenos %}
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import androidx.paging.Pager
import androidx.paging.PagingConfig
import androidx.paging.PagingData
import androidx.paging.cachedIn
import com.example.coroutine.paging.ArticlePagingSource
import com.example.coroutine.repository.ArticleItem
import kotlinx.coroutines.flow.Flow

class ArticleViewModelPage : ViewModel() {
  private val articles by lazy {
    Pager(
      config = PagingConfig(pageSize = 10),
      pagingSourceFactory = { ArticlePagingSource() }
    ).flow.cachedIn(viewModelScope)
  }
  fun loadArticle(): Flow<PagingData<ArticleItem>> = articles
}
{% endhighlight %}

## PageAdapter
使用getItem()取得資料。
{% highlight kotlin linenos %}
val item = getItem(position)
{% endhighlight %}

繼承PagingDataAdapter，建構子參數傳入DiffUtil。<br>
```
: PagingDataAdapter<ArticleItem, MyViewHolder>(DiffUtil)
```

DiffUtil 用來比對資料是否有更新，比對內容是否相同。<br>

{% highlight kotlin linenos %}
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.paging.PagingDataAdapter
import androidx.recyclerview.widget.DiffUtil
import com.example.coroutine.databinding.ArticleItemBinding
import com.example.coroutine.repository.ArticleItem

class ArticlePageAdapter :
  PagingDataAdapter<ArticleItem, MyViewHolder>(
    object : DiffUtil.ItemCallback<ArticleItem>() {
      override fun areItemsTheSame(
        oldItem: ArticleItem,
        newItem: ArticleItem
      ): Boolean {
        return oldItem.title == newItem.title
      }

      override fun areContentsTheSame(
        oldItem: ArticleItem,
        newItem: ArticleItem
      ): Boolean {
        return oldItem == newItem
      }
    }) {

  override fun onCreateViewHolder(
    parent: ViewGroup,
    viewType: Int
  ): MyViewHolder {
    val binding = ArticleItemBinding.inflate(LayoutInflater.from(parent.context), parent, false)
    return MyViewHolder(binding)
  }

  override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
    val item = getItem(position)
    item?.let{
      val binding = holder.binding as ArticleItemBinding
      binding.itemTv.text = "${item.title} "
    }
  }
}
{% endhighlight %}

### ViewHolder
{% highlight kotlin linenos %}
import androidx.recyclerview.widget.RecyclerView
import androidx.viewbinding.ViewBinding

class MyViewHolder(val binding: ViewBinding) :
  RecyclerView.ViewHolder(binding.root) {
}
{% endhighlight %}

## Activity
{% highlight kotlin linenos %}
import android.os.Bundle
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import androidx.recyclerview.widget.LinearLayoutManager
import com.example.coroutine.adapter.ArticlePageAdapter
import com.example.coroutine.adapter.LoadMoreAdapter
import com.example.coroutine.databinding.ActivityPageBinding
import com.example.coroutine.model.ArticleViewModelPage
import kotlinx.coroutines.launch

class PageActivity : AppCompatActivity() {
  private val articleViewModelPage by viewModels<ArticleViewModelPage>()
  private lateinit var binding : ActivityPageBinding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityPageBinding.inflate(layoutInflater)
    setContentView(binding.root)
    val articlePageAdapter = ArticlePageAdapter()
    binding.recyleView.layoutManager = LinearLayoutManager(this)
    binding.recyleView.adapter = articlePageAdapter
    lifecycleScope.launch {
      articleViewModelPage.loadArticle().collect { value ->
        articlePageAdapter.submitData(value)
      }
    }
  }
}
{% endhighlight %}

## xml

ViewHolder article_item.xml
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TextView
        android:id="@+id/item_tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

Activity RecyclerView
{% highlight css linenos %}
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.PageActivity">
        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyleView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

## LoadMore
LoadMoreAdapter
{% highlight kotlin linenos %}
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.paging.LoadState
import androidx.paging.LoadStateAdapter
import com.example.coroutine.databinding.LoadMoreBinding

class LoadMoreAdapter : LoadStateAdapter<MyViewHolder>() {
  override fun onBindViewHolder(
    holder: MyViewHolder,
    loadState: LoadState
  ) {
  }

  override fun onCreateViewHolder(
    parent: ViewGroup,
    loadState: LoadState
  ): MyViewHolder {
    val binding = LoadMoreBinding.inflate(LayoutInflater.from(parent.context), parent, false)
    return MyViewHolder(binding)
  }
}
{% endhighlight %}

{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ProgressBar
        android:id="@+id/progressBar"
        style="?android:attr/progressBarStyle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Loading ......."
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/progressBar" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

### Activity
adapter換成withLoadStateFooter()
{% highlight kotlin linenos %}
// 換成withLoadStateFooter
binding.recyleView.adapter = articlePageAdapter.withLoadStateFooter(LoadMoreAdapter())
{% endhighlight %}

{% highlight kotlin linenos %}
import android.os.Bundle
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import androidx.recyclerview.widget.LinearLayoutManager
import com.example.coroutine.adapter.ArticlePageAdapter
import com.example.coroutine.adapter.LoadMoreAdapter
import com.example.coroutine.databinding.ActivityPageBinding
import com.example.coroutine.model.ArticleViewModelPage
import kotlinx.coroutines.launch

class PageActivity : AppCompatActivity() {
  private val articleViewModelPage by viewModels<ArticleViewModelPage>()
  private lateinit var binding : ActivityPageBinding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityPageBinding.inflate(layoutInflater)
    setContentView(binding.root)
    val articlePageAdapter = ArticlePageAdapter()
    binding.recyleView.layoutManager = LinearLayoutManager(this)

    // 換成withLoadStateFooter
    binding.recyleView.adapter = articlePageAdapter.withLoadStateFooter(LoadMoreAdapter())
    lifecycleScope.launch {
      articleViewModelPage.loadArticle().collect { value ->
        articlePageAdapter.submitData(value)
      }
    }
  }
}
{% endhighlight %}