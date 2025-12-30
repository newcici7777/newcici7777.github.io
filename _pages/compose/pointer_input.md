---
title: pointerInput()
date: 2025-12-30
keywords: Android, Jetpack compose, pointerInput
---
pointerInput()可以診測各種手勢，以下以點擊作為例子:<br>
- onTap 點一下
- onDoubleTap 點二下
- onPress 壓
- onLongPress 長壓

## 取得點擊xy座標
![img]({{site.imgurl}}/compose/modifier/pointer_tap1.png)<br>

{% highlight kotlin linenos %}
@Composable
fun PointerInputExample1() {
  var lastOffset by remember { mutableStateOf(Offset.Zero) }
  Box(
    modifier = Modifier
      .size(100.dp)
      .pointerInput(Unit) {
        detectTapGestures(
          onTap = { offset ->
            lastOffset = offset
          }
        )
      }
  ) {
    Text("x = ${lastOffset.x.toInt()}, y = ${lastOffset.y.toInt()}")
  }
}
{% endhighlight %}

## 取得各個點擊手勢xy座標
{% highlight kotlin linenos %}
@Composable
fun PointerInputExample1() {
  var lastOffset by remember { mutableStateOf(Offset.Zero) }
  var gesture by remember { mutableStateOf("") }
  Box(
    modifier = Modifier
      .size(100.dp)
      .pointerInput(Unit) {
        detectTapGestures(
          onTap = { offset ->
            lastOffset = offset
            gesture = "onTap"
          },
          onDoubleTap = { offset ->
            lastOffset = offset
            gesture = "onDoubleTap"
          },
          onLongPress = { offset ->
            lastOffset = offset
            gesture = "onLongPress"
          },
          onPress = { offset ->
            lastOffset = offset
            gesture = "onPress"
          }
        )
      }
  ) {
    Text("gesture = $gesture x = ${lastOffset.x.toInt()}, y = ${lastOffset.y.toInt()}")
  }
}
{% endhighlight %}