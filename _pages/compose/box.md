---
title: Box
date: 2023-05-03
keywords: Android, Jetpack compose, Box
---
![img]({{site.imgurl}}/compose/compose_box1.png)
若沒任何組件，默認是堆疊佈局  
寫在後面會把前面給疊上  
box疊在起來  
{% highlight kotlin linenos %}
Box(){
    Box(modifier = Modifier.size(200.dp).background(Color.Red)) //前面
    Box(modifier = Modifier.size(100.dp).background(Color.Green)) //後面
}
{% endhighlight %}
置中  
`.align(Alignment.Center)`
{% highlight kotlin linenos %}
Box() {
    Box(
        modifier = Modifier
            .size(200.dp)
            .background(Color.Red)
    )
    Box(modifier = Modifier
        .align(Alignment.Center)
        .size(100.dp)
        .background(Color.Green))
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/compose_box2.png)