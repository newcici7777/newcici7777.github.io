---
title: Retrofit StateFlow ViewModel
date: 2025-10-14
keywords: kotlin, StateFlow, MutableStateFlow, Retrofit, ViewModel
---
使用前面文章retrofit、stateFlow、viewModel、lifecycleScope、viewModelScope

將MutableStateFlow的類型改為ArticleList。<br>

在viewModelScope協程，呼叫網路連線。<br>
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

Activity呼叫viewModel.getArticleList()。<br>
lifecycleScope中viewModel._articleList.collect()收集接收網路回傳的資料。<br>
{% highlight kotlin linenos %}
class MainActivity09 : AppCompatActivity() {
  private val viewModel by viewModels<ArticleViewModel>()
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)
    lifecycleScope.launch {
      viewModel._articleList.collect { value ->
        textv.text = value.toString()
      }
    }
    submit.setOnClickListener {
      viewModel.getArticleList()
    }
  }
}
{% endhighlight %}

Activity不直接面對Retrofit的網路連線，由ViewModel去面對Retrofit的網路連線，Activity只跟ViewModel溝通。

ViewModel處理網路資料的接收，與收到資料後，就發射資料，而Activity會使用collect()監視並接收ViewModel傳來的資料。<br>

- viewModel.getArticleList()
- viewModel._articleList.collect()