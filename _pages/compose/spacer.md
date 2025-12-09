---
title: Spacer spacedBy
date: 2025-12-08
keywords: Android, Jetpack compose, Spacer, spacedBy
---
Prerequisites:

- [row][1]

## Spacer
Spacer是建立空格。<br>

### 建立寬度Spacer
建立寬度的空格
```
Spacer(modifier = Modifier.width(8.dp))
```

下面的例子，每一個Text之間都有一個空格。<br>
![img]({{site.imgurl}}/compose/modifier/spacer1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testSpacer() {
  Row {
    Text("abcd")
    Spacer(modifier = Modifier.width(8.dp))
    Text("fegd")
    Spacer(modifier = Modifier.width(16.dp))
    Text("gggg")
  }
}
{% endhighlight %}

### 建立高度Spacer
建立高度的空格。
```
Spacer(modifier = Modifier.height(8.dp))
```

下面的例子，每一個Text之間都有一個空格。<br>

![img]({{site.imgurl}}/compose/modifier/spacer2.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testSpacer() {
  Column {
    Text("abcd")
    Spacer(modifier = Modifier.height(8.dp))
    Text("fegd")
    Spacer(modifier = Modifier.height(16.dp))
    Text("gggg")
  }
}
{% endhighlight %}

## spacedBy 間隔
語法
```
Arrangement.spacedBy(間隔大小.dp)
Arrangement.spacedBy(16.dp)
```
每一個Child元素，都有16.dp大小的間隔。<br>
{% highlight kotlin linenos %}
@Composable
fun testSpacer() {
  Row(
    modifier = Modifier.size(width = 200.dp, height = 200.dp),
    horizontalArrangement = Arrangement.spacedBy(16.dp)
  ) {
    Text("abcd")
    Text("fegd")
    Text("gggg")
  }
}
{% endhighlight %}

[1]: {% link _pages/compose/row.md %}