---
title: clip圓角
date: 2023-05-03
keywords: Android, Jetpack compose, clip圓角
---
Clip圓角的作用區域
{% highlight kotlin linenos %}
.clip(RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp))//頂端二邊有圓角
{% endhighlight %}
要放在
```
.fillMaxSize()與.background()之前
```
{% highlight kotlin linenos %}
Column(modifier = Modifier
    .clip(RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp))//頂端二邊有圓角
    .fillMaxSize() //填滿
    .background(Color.White)//白色
){
Text(text = "明細")
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/clip.png)   