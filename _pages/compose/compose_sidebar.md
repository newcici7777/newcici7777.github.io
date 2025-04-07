---
title: Compose sidebar
date: 2023-05-03
keywords: Android, Jetpack compose, Compose sidebar
---
![img]({{site.imgurl}}/compose/sidebar1.png)
{% highlight kotlin linenos %}
@Composable
fun SliderSample() {
    var values by remember{
        mutableStateOf(0f)
    }
    Slider(value = values, onValueChange = {
        values = it
    }, valueRange = 0f..100f, steps = 4)
}
{% endhighlight %}

可以選擇一個範圍
![img]({{site.imgurl}}/compose/sidebar2.png)
預設mutableStateOf的範圍為0f..1f
{% highlight kotlin linenos %}
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun SliderSample() {
    var values by remember {
        mutableStateOf(0.2f..0.8f)
    }
    RangeSlider(value = values, onValueChange ={
        values = it
    } )
}
{% endhighlight %}