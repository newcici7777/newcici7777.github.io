---
title: Row
date: 2023-05-03
keywords: Android, Jetpack compose, Row
---
{% highlight kotlin linenos %}
Row(
  modifier = Modifier
    .size(200.dp)
    .background(Color.Green)
){
  Text(
    "Column First Item",
    modifier = Modifier.weight(1f, true)
  )
  Text("Column Second Item",
    modifier = Modifier.weight(1f, true))
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/row1.png)   