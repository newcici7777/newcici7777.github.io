---
title: Change status bar height
date: 2023-05-29
keywords: Android, Jetpack compose, Change status bar height
---
參考此篇
[https://stackoverflow.com/questions/73455192/android-jetpack-compose-statusbar-height](https://stackoverflow.com/questions/73455192/android-jetpack-compose-statusbar-height)
![img]({{site.imgurl}}/compose/status_bar_height1.png)
{% highlight kotlin linenos %}
var systemUiController = rememberSystemUiController()
LaunchedEffect(key1 = Unit) {
    //第二個參數可以設置系統列上的文字顏色，先設置默認
    systemUiController.setStatusBarColor(Color.Transparent)
}
//標題欄高度
val appBarHeight = 56.dp
{% endhighlight %}
![img]({{site.imgurl}}/compose/status_bar_height2.png)
{% highlight kotlin linenos %}
Row(
    modifier = Modifier
        .background(
            Brush.linearGradient(
                listOf(
                    Blue700,
                    Blue200,
                )
            )
        )
        .fillMaxWidth() //設最寬
        .height(appBarHeight + statusBarHeightDp) //設高度
        //Modifier.padding(all: Dp) 要一個類型dp的參數
        .padding(top = statusBarHeightDp)
        .then(modifier) //then的功能是把不同的modifier合併 top bar的modifier跟row的modifier合併
    ,
    //標題垂直置中
    horizontalArrangement = Arrangement.Center,
    verticalAlignment = Alignment.CenterVertically
) {
    content()
}
{% endhighlight %}