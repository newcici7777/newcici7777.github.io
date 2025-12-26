---
title: updateTransition
date: 2025-12-26
keywords: Android, Jetpack compose, updateTransition
---
updateTransition 會根據狀態改變，不同的狀態不同變化，可以有多個動畫「同時」組合一起，開始時間與結束時間都一樣。<br>

與[graphicsLayer和animateFloatAsState][1]十分相像。<br>
但animateAsState是各個「獨立」動畫，組合在一起，每一個執行時間可以不一樣。<br>

語法:
```
// 狀態
var isExpanded by remember { mutableStateOf(false) }

val transition = updateTransition(
    targetState = isExpanded,  // ← 當狀態改變時，動畫開始
)
```

{% highlight kotlin linenos %}
@Composable
fun UpdateTransitionExample1() {
  // 狀態
  var isVisible by remember { mutableStateOf(false) }
  val updateTransit = updateTransition(
    // 當觀察的狀態
    targetState = isVisible
  )

  val height by updateTransit
    .animateDp(label = "height animate") { state ->
      // 當狀態改變 就會執行這裡
      if (state) 200.dp else 100.dp
    }
  val color by updateTransit
    .animateColor(label = "color animate") { state ->
      // 當狀態改變 就會執行這裡
      if (state) Color.Green else Color.Red
    }
  val alpha by updateTransit
    .animateFloat(label = "alpha animate") { state ->
      // 當狀態改變 就會執行這裡
      if (state) 1f else 0f
    }
  Box(
    modifier = Modifier
      .height(height)
      .fillMaxWidth()
      .background(color)
      .clickable(onClick = {
        isVisible = !isVisible
      })
  ) {
    Text("Hello", modifier = Modifier.alpha(alpha))
  }
}
{% endhighlight %}

初始狀態：isExpanded = false<br>
點擊按鈕：isExpanded = true<br>

執行順序：<br>
1. updateTransition 檢測到 targetState 從 false → true
2. 同時啟動三個動畫：
   - cardHeight: 100dp → 200dp
   - cardColor: Red → Green  
   - textAlpha: 0f → 1f
3. 所有動畫同步進行，同時開始同時結束



[1]: {% link _pages/compose/animate_as_state.md %}