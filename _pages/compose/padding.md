---
title: padding
date: 2023-05-03
keywords: Android, Jetpack compose, padding
---
{% highlight kotlin linenos %}
Column(
  modifier = Modifier
  .fillMaxSize() //填滿
  .background(Color.White)//白色
  .padding(top = 8.dp) //與上面的元間有8dp的距離
  .padding(horizontal = 8.dp, vertical = 8.dp) //與左邊及下面有8dp距離
) 
{% endhighlight %}