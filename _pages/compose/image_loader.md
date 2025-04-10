---
title: ImageLoader
date: 2023-05-03
keywords: Android, Jetpack compose, ImageLoader
---
參考以下網址:
<https://github.com/coil-kt/coil#jetpack-compose>  
<https://developer.android.com/jetpack/compose/graphics/images/loading?hl=zh-cn>  

gradle匯入  
{% highlight groovy linenos %}
implementation("io.coil-kt:coil-compose:2.4.0")
{% endhighlight %}

記得在AndroidManifest加上權限  
{% highlight html linenos %}
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
{% endhighlight %}

把圖片匯入
{% highlight kotlin linenos %}
AsyncImage(
  model = "https://example.com/image.jpg",
  contentDescription = null,
)
{% endhighlight %}
