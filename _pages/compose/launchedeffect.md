---
title: LaunchedEffect
date: 2025-12-25
keywords: Android, Jetpack compose, LaunchedEffect
---
## 一開始進入
Prerequisites:

- [List][1]

LaunchedEffect是當「一開始進入」Compose組件時，會呼叫的區塊，可在此區塊處理耗時操作，不會對Compose產生阻塞。<br>

語法<br>
```
LaunchedEffect(Unit) {  
	// 要做的事
}
```
「一開始進入」Compose組件時，會呼叫的LaunchedEffect區塊，並產生500筆資料塞入list中。<br>

![img]({{site.imgurl}}/compose/launchedeffect1.png)<br>

完整程式碼
{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample6() {
  val list = remember { mutableStateListOf<String>() }
  LaunchedEffect(Unit) {
    list.addAll(List(500) { "Item $it" })
  }
  LazyColumn{
    items(list) { item ->
      Text("$item")
    }
  }
}
{% endhighlight %}

## 狀態改變
當狀態改變時，就會執行Lambda區塊`{}`。
```
LaunchedEffect(狀態) {  
	// 要做的事
}
```

{% highlight kotlin linenos %}
  var isExpanded by remember { mutableStateOf(false) }
  // 當 isExpanded 有變化，就會執行花括號{}中要做的事。
  LaunchedEffect(isExpanded) {
    scale.animateTo(
      targetValue = if (isExpanded) 2f else 1f,
      animationSpec = tween(5000)
    )
  }
{% endhighlight %}

[1]: {% link _pages/kotlin/list.md %}#其它建立List的方式