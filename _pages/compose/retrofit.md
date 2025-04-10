---
title: Retrofit
date: 2023-05-03
keywords: Android, Jetpack compose, Retrofit
---
grandle import以下內容
{% highlight groovy linenos %}
def retrofit = "2.9.0"
implementation "com.squareup.retrofit2:retrofit:$retrofit"
implementation "com.squareup.retrofit2:converter-gson:$retrofit"
implementation "com.squareup.retrofit2:converter-moshi:$retrofit"
def moshi_version = "1.13.0"
implementation "com.squareup.moshi:moshi-kotlin:$moshi_version"
{% endhighlight %}

![img]({{site.imgurl}}/compose/retrofit1.png)  
{% highlight kotlin linenos %}
import com.squareup.moshi.Moshi
import com.squareup.moshi.kotlin.reflect.KotlinJsonAdapterFactory
import retrofit2.Retrofit
import retrofit2.converter.moshi.MoshiConverterFactory
object Network {
    //文件位置:https://docs.apipost.cn/preview/1a28e17fa3c8f473/16838456ae6dc5c7
    private const val baseUrl =
        "https://mock.apipost.cn/app/mock/project/ced69cf2-9206-4a42-895e-dd7442a888df/"
    //創建一個Retrofit builder
    private val retrofit = Retrofit
        .Builder()
        .baseUrl(baseUrl)
        .addConverterFactory(
						//建立Moshi工廠
            MoshiConverterFactory.create(
                Moshi.Builder()
                    .add(KotlinJsonAdapterFactory())
                    .build()
            )
        ).build()
    //傳進來什麼類型，返回就是什麼類型
    fun <T> createService(clazz: Class<T>):T {
        return retrofit.create(clazz)
    }
}
{% endhighlight %}

圖中先建立ArticleService  
先建一個interface 在Service的目錄下  
![img]({{site.imgurl}}/compose/retrofit2.png)  

生成伴生物件  
![img]({{site.imgurl}}/compose/retrofit3.png)  
![img]({{site.imgurl}}/compose/retrofit4.png)  

進到<https://docs.apipost.cn/preview/1a28e17fa3c8f473/16838456ae6dc5c7>
點進文章列表  
使用/article/list  
參數有pageOffset跟pageSize，如下圖  
![img]({{site.imgurl}}/compose/retrofit5.png)
{% highlight kotlin linenos %}
@GET("article/list")
suspend fun list(
    @Query("pageOffset") pageOffset: Int,
    @Query("pageSize") pageSize: Int
)
{% endhighlight %}  

![img]({{site.imgurl}}/compose/retrofit6.png)  

建立基礎的Response
{% highlight kotlin linenos %}
package com.example.project1.model.entity
open class BaseResponse {
    var code:Int = -1
    var message:String = ""
}
{% endhighlight %} 

![img]({{site.imgurl}}/compose/retrofit7.png)  

建立繼承的Response
![img]({{site.imgurl}}/compose/retrofit8.png)  
{% highlight kotlin linenos %}
import com.squareup.moshi.Json
import com.squareup.moshi.JsonClass
@JsonClass(generateAdapter = true)
data class ArticleEntity(
    val title: String,
    var source: String,
    @Json(name = "time")
    var timestamp: String
)
//因為data有可能為空，所以類型設空值List<ArticleEntity>?
data class ArticleListResponse(val data: List<ArticleEntity>?):BaseResponse()
{% endhighlight %} 

![img]({{site.imgurl}}/compose/retrofit9.png) 

回到Article Service

繼承:ArticleListResponse 
{% highlight kotlin linenos %}
import com.example.project1.model.entity.ArticleListResponse
import retrofit2.http.GET
import retrofit2.http.Query
interface ArticleService {
    @GET("article/list")
    suspend fun list(
        @Query("pageOffset") pageOffset: Int,
        @Query("pageSize") pageSize: Int
    ): ArticleListResponse

    companion object {
        //創建Home Service的實例
        fun instance(): ArticleService {
            return Network.createService(ArticleService::class.java)
        }
    }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/retrofit10.png)  
在viewmodel中使用
{% highlight kotlin linenos %}
class ArticleViewModel : ViewModel() {
    private val articleService = ArticleService.instance()
    val pageSize = 10 //每頁10筆
    private var pageOffset = 1 //預設從第1頁讀取
    var list by mutableStateOf(
        listOf(
   
        ArticleEntity(
            title = "test3",
            source = "source3",
            timestamp = "2023-01-01"
        ),
        ArticleEntity(
            title = "test4",
            source = "source4",
            timestamp = "2023-01-01"
        )
    )
    )
        private  set //不讓人修改

    var listLoaded by mutableStateOf(false)
    private set

    suspend fun fetchArticleList(){
       val res = articleService.list(pageOffset = pageOffset, pageSize = pageSize)
        if(res.code == 0 && res.data != null) {
            list = res.data
            listLoaded = true //是否載入完畢
        }
    }
}
{% endhighlight %}

在screen畫面使用

在LaunchedEffect使用(fetchArticleList)
![img]({{site.imgurl}}/compose/retrofit11.png)
因為有”是否載入完畢”，要做placeholder
{% highlight kotlin linenos %}
fun ArticleItem(article: ArticleEntity, loaded: Boolean, modifier: Modifier = Modifier) {
    Column(modifier = modifier.padding(8.dp)) {
        Text(
            article.title,
            color = Color(0xFF333333),
            fontSize = 16.sp,
            maxLines = 2,
            overflow = TextOverflow.Ellipsis,
            modifier = Modifier
                .padding(bottom = 8.dp)
                .placeholder(
                    visible = !loaded,
                    color = Color.Gray,
                    highlight = PlaceholderHighlight.shimmer(highlightColor = Color.Gray)
                )
        )
        Row(horizontalArrangement = Arrangement.SpaceBetween,
            modifier = Modifier
                .fillMaxWidth()
                .placeholder(
                    visible = !loaded,
                    color = Color.Gray,
                    highlight = PlaceholderHighlight.shimmer(highlightColor = Color.Gray)
                )) {
            Text(
                text = "來源:${article.source}",
                color = Color(0xFF999999),
                fontSize = 10.sp,
                maxLines = 1,
                overflow = TextOverflow.Ellipsis
            )
            Text(text = article.timestamp)
        }
        Spacer(Modifier.height(8.dp))
        Divider()
    }
}
{% endhighlight %}

在呼叫ArticleItem時添加loaded
{% highlight kotlin linenos %}
if (vm.showArticleList) {
  //列表
  items(articleViewModel.list) { article ->
      ArticleItem(article,
          articleViewModel.listLoaded,
          modifier = Modifier.clickable {
              //Log.d("======","here")
              onNavigateToArticle.invoke()
          })
  }
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/retrofit12.png)  