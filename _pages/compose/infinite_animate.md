---
title: InfiniteTransition 無限循環動畫
date: 2025-12-26
keywords: Android, Jetpack compose, InfiniteTransition
---
語法
```
  // 建立無限循環動畫
  val infinite = rememberInfiniteTransition()
  val 變數 by infinite.animateXXX(
    initialValue = 動畫開始的值,
    targetValue = 動畫要達到的目標值,
    animationSpec = infiniteRepeatable(
      animation = 要重複的動畫,
      repeatMode = 反覆播放模式
    )
  )
```

repeatMode
```
RepeatMode.Reverse  // 來回播放：A → B → A → B...
RepeatMode.Restart  // 重新開始：A → B → 跳回A → B...
```

以下例子<br>
動畫會在 0.5 → 2.0 → 0.5 → 2.0... 之間無限循環。<br>

{% highlight kotlin linenos %}
@Composable
fun InfiniteTransitionExample() {
  val infinite = rememberInfiniteTransition()
  val scale by infinite.animateFloat(
    initialValue = 0.5f,
    targetValue = 2f,
    animationSpec = infiniteRepeatable(
      animation = tween(1000),
      repeatMode = RepeatMode.Reverse
    )
  )
  Box(modifier = Modifier
    .size(100.dp)
    .scale(scale)
    .background(Color.Blue)
  )
}
{% endhighlight %}