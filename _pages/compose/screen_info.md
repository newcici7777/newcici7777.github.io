---
title: 瑩幕寬高
date: 2023-05-03
keywords: Android, Jetpack compose, screenWidth, screenHeight
---
{% highlight kotlin linenos %}
var screenWidth :Float
var screenHeight :Float
with(LocalDensity.current){
    screenWidth = LocalConfiguration.current.screenWidthDp.dp.toPx()
    screenHeight = LocalConfiguration.current.screenHeightDp.dp.toPx()
}
{% endhighlight %}