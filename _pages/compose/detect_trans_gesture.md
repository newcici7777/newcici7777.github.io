---
title: detectTransformGestures
date: 2025-12-31
keywords: Android, Jetpack compose, detectTransformGestures
---

參數:
- gestureZoom 放大縮小比例
- gestureCentroid 取得中心點
- gestureRotation 旋轉
- gesturePan 水平移動、垂直移動

## 放大縮小
{% highlight kotlin linenos %}
@Composable
fun detectTransform() {
  var scale by remember { mutableStateOf(1f) }
  Box(
    modifier = Modifier
      .fillMaxSize()
      .pointerInput(Unit) {
        detectTransformGestures(panZoomLock = false) {
            gestureCentroid, gesturePan, gestureZoom, gestureRotation ->
            // 原本的尺寸 * 放大縮小的尺寸
            // coerceIn(縮放最小值, 縮放最大值)
            // 縮小不能小於0.5f 放大不能超過3倍
          scale = (scale * gestureZoom).coerceIn(0.5f, 3f)
        }
      }
  ) {
    Column {
      Box(modifier = Modifier
        .size(200.dp)
        // 設定放大縮小
        .graphicsLayer {
          scaleX = scale
          scaleY = scale
        }
        .background(Color.Blue)
      )
    }
  }
}
{% endhighlight %}

## 