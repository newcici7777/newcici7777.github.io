---
title: 圓形圖片
date: 2023-05-03
keywords: Android, Jetpack compose, 圓形圖片
---
圓角主要由以下二個部分製成
```
.clip(CircleShape)
contentScale = ContentScale.Crop,    
```
{% highlight kotlin linenos %}
Image(
painter = painterResource(id = R.drawable.*img1*),    
contentDescription = null,    
contentScale = ContentScale.Crop,    
modifier = Modifier        
.size(62.dp)        
.clip(CircleShape)
)
{% endhighlight %}
![img]({{site.imgurl}}/compose/round_pic.png)   