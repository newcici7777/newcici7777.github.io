---
title: Spacer spacedBy
date: 2025-12-08
keywords: Android, Jetpack compose, Spacer, spacedBy
---
Prerequisites:

- [row][1]

## 語法
Spacer是建立空格。<br>

建立寬度的空格。
```
Spacer(modifier = Modifier.width(8.dp))
```

建立高度的空格。
```
Spacer(modifier = Modifier.height(8.dp))
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

[1]: {% link _pages/compose/row.md %}