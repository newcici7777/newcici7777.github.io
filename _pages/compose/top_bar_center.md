---
title: Topbar置中
date: 2023-05-03
keywords: Android, Jetpack compose, Topbar center
---
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
      },)
  },
  modifier = Modifier
      .background(MaterialTheme.colors.primary)
      .statusBarsPadding()
) {
  Text(text = "文章詳情")
}
{% endhighlight %}
