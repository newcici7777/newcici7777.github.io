---
title: App Top bar添加上一頁跟action的按鈕
date: 2023-05-03
keywords: Android, Jetpack compose, Topbar back
---
透過navigationIcon  
透過actions  
{% highlight kotlin linenos %}
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
{% endhighlight %}