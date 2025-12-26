---
title: pointerInput dragAmount
date: 2025-12-26
keywords: Android, Jetpack compose, pointerInput, dragAmount, detectDragGestures
---
Prerequisites:

- [IntOffset][1]
- [draggable][2]

## dragAmount
dragAmount跟draggable一樣，都是拖曳的「差距」，不同的是，dragAmount有提供x軸、y軸，不用像draggable需要指定orientation是水平或垂直。

```
dragAmount.x
dragAmount.y
```

完整程式碼:
{% highlight kotlin linenos %}
@Composable
fun PointInputExample1() {
  var offsetX by remember { mutableStateOf(0f) }
  Column {
    Box(
      modifier = Modifier
        .size(100.dp)
        .offset { IntOffset(offsetX.roundToInt(), 0) }
        // background() 一定要放在offset之後
        .background(Color.Blue)
        .pointerInput(Unit) {
          detectDragGestures { change, dragAmount ->
            offsetX += dragAmount.x
          }
        }
    )
  }
}
{% endhighlight %}

[1]: {% link _pages/compose/intoffset.md %}
[2]: {% link _pages/compose/draggable.md %}
