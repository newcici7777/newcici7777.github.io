---
title: 自制App Top Bar
date: 2023-05-03
keywords: Android, Jetpack compose, Topbar 
---
{% highlight kotlin linenos %}
Row(
  modifier =Modifier
    .statusBarsPadding() //系統列
    .height(appBarHeight),//appbar的高度
  verticalAlignment = Alignment.CenterVertically//垂直置中
){
Text(
    "TaskContent",
    modifier = Modifier.fillMaxWidth(),//一定要設定不然不能居中
    textAlign = TextAlign.Center,//置中
    color = Color.White,
    fontSize = 18.sp
)
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/top_bar.png) 