---
title: Navigation 回到上一頁轉場
date: 2023-05-03
keywords: Android, Jetpack compose, Navigation back
---

先參考之前的AnimationNavigation的文章

還有App Top bar添加上一頁的文章

![img]({{site.imgurl}}/compose/nav_back.png) 

在NavHost加上navController.popBackStack()
{% highlight kotlin linenos %}
composable(Destinations.ArticleDetail.route) {
  ArticleDetailScreen(onBack = {
    navController.popBackStack()
  })
 }
{% endhighlight %}

在詳細頁添加  
fun()中丟入lambda的參數onBack: () -> Unit  
在navigation的.clickable加上onBack()，記得要尾部括號()，不然不會執行lambda  
{% highlight kotlin linenos %}
fun ArticleDetailScreen(onBack: () -> Unit) {
  Scaffold(
    topBar = {
      TopAppBar(
        title = {
          Text(
            text = "Detail",
            modifier = Modifier.fillMaxWidth(),
            textAlign = TextAlign.Center
          )
        },
        navigationIcon = {
          Icon(
            imageVector = Icons.Default.NavigateBefore,
            contentDescription = null,
            //增加返回的方法
            modifier = Modifier
              .clickable {
                onBack()
              }
              .padding(8.dp)
          )
        },
        actions = {
          Icon(imageVector = Icons.Default.TextFields,
            contentDescription = null,
            modifier = Modifier
              .clickable {
                //TODO 字體放大
              }
              .padding(8.dp)
          )
        }
      )
    },
    modifier = Modifier
      .background(MaterialTheme.colors.primary)
      .statusBarsPadding()
  ) {
    Text(text = "文章詳情")
  }
}
{% endhighlight %}