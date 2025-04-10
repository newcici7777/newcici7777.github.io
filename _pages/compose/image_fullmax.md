---
title: 圖片全瑩幕
date: 2023-05-03
keywords: Android, Jetpack compose, image, full max size
---
圖片尚未擴張前  
![img]({{site.imgurl}}/compose/full_size_img1.png) 
{% highlight kotlin linenos %}
Box(modifier = Modifier.fillMaxSize()) {
  Image(
    painter = painterResource(id = R.drawable.img1),
    contentDescription = null,
    modifier = Modifier.fillMaxSize()
  )
 }
{% endhighlight %}
 
圖片尚未擴張後   
![img]({{site.imgurl}}/compose/full_size_img2.png)  
{% highlight kotlin linenos %}
Box(modifier = Modifier.fillMaxSize()) {
	Image(
		painter = painterResource(id = R.drawable.img1),
		contentDescription = null,
		modifier = Modifier.fillMaxSize(),
		contentScale = ContentScale.Crop
	)
}
{% endhighlight %}