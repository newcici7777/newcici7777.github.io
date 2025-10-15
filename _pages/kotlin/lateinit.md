---
title: lateinit
date: 2025-10-15
keywords: kotlin, lateinit
---
不用一開始就要初始化變數，使用的時候，才初始化。<br>

原本這樣寫會出錯，強迫一定要初始化
{% highlight kotlin linenos %}
private var binding : ActivityMainBinding
{% endhighlight %}

在var前面加上lateinit延後初始化，就不用一開始就一定要初始化。<br>
{% highlight kotlin linenos %}
private lateinit var binding : ActivityMainBinding
{% endhighlight %}