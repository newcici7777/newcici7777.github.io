---
title: Compose mutableStateOf
date: 2023-05-03
keywords: Android, Jetpack compose, mutableStateOf, by remeber
---
如果只使用var count = 1; 變數再重新繪製手機介面時就又變成1，所以要使用 by remeber

以下count為mutable類別，要使用count.value讀取
{% highlight kotlin linenos %}
var count = remember {
        mutableStateOf(1)
    }
Text(text = "xx${count.value}xxx", Modifier.clickable {
        count.value++
        Log.d("xxxx","I comin")
    } )
{% endhighlight %}

以下count為int類別，就不用count.value
{% highlight kotlin linenos %}
    var count by remember {
        mutableStateOf(1)
    }
{% endhighlight %}

{% highlight kotlin linenos %}
@Composable
fun StateSample() {
    var count by remember {
        mutableStateOf(1)
    }
    Log.d("xxxx","outter${count}")
    Text(text = "xx${count}xxx", Modifier.clickable {
        count++
        Log.d("xxxx","I comin")
    } )
}
{% endhighlight %}