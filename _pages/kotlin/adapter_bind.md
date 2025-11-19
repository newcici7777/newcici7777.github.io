---
title: Adapter ViewBinding
date: 2025-10-17
keywords: kotlin, Adapter ViewBinding
---
## xml
### Activity RecyclerView
以下是Activity xml，有一個RecyclerView。<br>

activity_main3.xml
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyleView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

### adapter xml
注意!!!!!! ConstraintLayout 的 layout_height 一定要是wrap_content，TextView 的 layout_height 也是 wrap_content 。<br>
之前測試的時候，列表的資料只有三筆，但永遠清單只有一筆資料，原因就是高度設為match_parent。<br>

article_item.xml
{% highlight kotlin linenos %}
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

## LayoutManager

LayoutManager：控制 RecyclerView 項目的排列方式。<br>

ConstraintLayout：是 item XML 中的視圖容器。<br>

不能因為item的layout是ConstraintLayout，就把LayoutManager設為ConstraintLayout，二者不是相同的東西。<br>

垂直
{% highlight kotlin linenos %}
binding.recyleView.layoutManager = LinearLayoutManager(this)
{% endhighlight %}

水平
{% highlight kotlin linenos %}
binding.recyleView.layoutManager = LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false)
{% endhighlight %}

網格 
{% highlight kotlin linenos %}
binding.recyleView.layoutManager = GridLayoutManager(this, 2)
{% endhighlight %}

## Activity 呼叫 Adapter
- 在 XML 中定義 RecyclerView
- 在 Activity 中初始化 Adapter
- 設置 LayoutManager
- 將 Adapter 設置給 RecyclerView
- 提供資料給 Adapter

### Main Activity
{% highlight kotlin linenos %}
import com.example.coroutine.databinding.ActivityMain3Binding
import com.example.coroutine.model.ArticleViewModel
import kotlinx.coroutines.launch
import kotlin.getValue

class MainActivity15 : AppCompatActivity() {
  private val viewModel by viewModels<ArticleViewModel>()
  private lateinit var binding : ActivityMain3Binding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMain3Binding.inflate(layoutInflater)
    setContentView(binding.root)
    val adapter = ArticleItemAdapter()
    binding.recyleView.layoutManager = LinearLayoutManager(this)
    binding.recyleView.adapter = adapter
    val articles = listOf(
      ArticleItem("標題1", "來源1", "時間1"),
      ArticleItem("標題2", "來源2", "時間2"),
      ArticleItem("標題3", "來源3", "時間3")
    )
    adapter.setData(articles)
  }
}
{% endhighlight %}

### ViewHolder
寫一個ViewHolder大家都可以共用。<br>
{% highlight kotlin linenos %}
import androidx.recyclerview.widget.RecyclerView
import androidx.viewbinding.ViewBinding

class MyViewHolder(val binding: ViewBinding) :
  RecyclerView.ViewHolder(binding.root) {
}
{% endhighlight %}

### Adapter

Android 官方文件說：
```
Adapter & ViewHolder should always use parent.context
不要自己傳 context，也不要用 Activity context
```
所以這邊的context使用的是`parent.context`
{% highlight kotlin linenos %}
    val binding = ArticleItemBinding.inflate(
    LayoutInflater.from(parent.context), parent, false)
{% endhighlight %}

完整ArticleItemAdapter
{% highlight kotlin linenos %}
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.recyclerview.widget.RecyclerView
import com.example.coroutine.databinding.ArticleItemBinding
import com.example.coroutine.repository.ArticleItem

class ArticleItemAdapter : RecyclerView.Adapter<MyViewHolder>() {
  private val data = ArrayList<ArticleItem>()
  fun setData(data: List<ArticleItem>) {
    this.data.clear()
    this.data.addAll(data)
    notifyDataSetChanged()
  }

  override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
    val binding = ArticleItemBinding.inflate(LayoutInflater.from(parent.context), parent, false)
    return MyViewHolder(binding)
  }

  override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
    val item = data[position]
    val binding = holder.binding as ArticleItemBinding
    binding.itemTv.text = "${item.title} "
  }

  override fun getItemCount(): Int {
    return data.size
  }
}
{% endhighlight %}

## 結合 stateflow 與 retrofit

### viewmodel
{% highlight kotlin linenos %}
import androidx.lifecycle.ViewModel
import com.example.coroutine.api.ArticleList
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import androidx.lifecycle.viewModelScope
import com.example.coroutine.api.ArticleApi
import kotlinx.coroutines.launch

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

### retrofit 
{% highlight kotlin linenos %}
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

### ArticleApi.kt 
{% highlight kotlin linenos %}
data class ArticleItem(val title: String, val source: String, val time: String)
{% endhighlight %}

{% highlight kotlin linenos %}
data class ArticleList(val data: List<ArticleItem>, val code: Int = -1, val message: String)
{% endhighlight %}

{% highlight kotlin linenos %}
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

### Activity
{% highlight kotlin linenos %}
import com.example.coroutine.databinding.ActivityMain3Binding
import com.example.coroutine.model.ArticleViewModel
import kotlinx.coroutines.launch
import kotlin.getValue

class MainActivity15 : AppCompatActivity() {
  private val viewModel by viewModels<ArticleViewModel>()
  private lateinit var binding : ActivityMain3Binding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMain3Binding.inflate(layoutInflater)
    setContentView(binding.root)
    val adapter = ArticleItemAdapter()
    binding.recyleView.layoutManager = LinearLayoutManager(this)
    binding.recyleView.adapter = adapter

    lifecycleScope.launch {
      viewModel.articleList.collect { value ->
        value?.data?.let {
          adapter.setData(value.data)
        }
      }
    }
  }
}
{% endhighlight %}