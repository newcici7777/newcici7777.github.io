---
title: clickable()
date: 2025-12-09
keywords: Android, Jetpack compose, clickable
---
語法
```
Modifier.clickable(
	onClick = {
        println("Box click")
    }
)
```
要使用模擬器去執行，不能使用Preview去執行。<br>

{% highlight kotlin linenos %}
@Composable
fun testClick() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(Color.Blue)
      .clickable(onClick = {
        println("Box click")
      })
  ) {
    Text("Click Test")
  }
}
{% endhighlight %}