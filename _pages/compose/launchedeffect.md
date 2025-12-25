---
title: LaunchedEffect
date: 2025-12-25
keywords: Android, Jetpack compose, LaunchedEffect
---
## 一開始進入
LaunchedEffect是當「一開始進入」Compose組件時，會呼叫的區塊，可在此區塊處理耗時操作，不會對Compose產生阻塞。

語法
```

```

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