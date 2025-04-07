---
title: Modifier
date: 2023-05-03
keywords: Android, Jetpack compose, Modifier
---
border/background/padding放置的位置不同，會有不同效果
{% highlight kotlin linenos %}
@Composable
fun ModifierSample() {
    Text(
        text = "text",
        modifier = Modifier
            .border(1.dp, Color.Red)
            .background(Color.Yellow)
            .padding(8.dp)
            .clickable {
                Log.i("-----","click me")
            }
    )
}
{% endhighlight %}