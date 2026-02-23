---
title: detectHorizontalDragGestures
date: 2025-12-31
keywords: Android, Jetpack compose, detectTransformGestures
---
## 語法
{% highlight kotlin linenos %}
suspend fun PointerInputScope.detectHorizontalDragGestures(
    onDragStart: (Offset) -> Unit = {},
    onDragEnd: () -> Unit = {},
    onDragCancel: () -> Unit = {},
    onHorizontalDrag: (change: PointerInputChange, dragAmount: Float) -> Unit
)
{% endhighlight %}

|參數|	觸發時機|	參數|	常見用途|
|:-----|:-------|:-------|:-------------|
|onDragStart|	手指開始拖拽時|	Offset（起始位置）|	初始化狀態、記錄起始點|
|onHorizontalDrag|	手指水平移動時|	change, dragAmount|	更新UI位置、處理拖拽邏輯|
|onDragEnd|	手指正常抬起時|	無|	完成操作、觸發動畫|
|onDragCancel|	拖拽被中斷時|	無|	恢復狀態、取消操作|

{% highlight kotlin linenos %}
@Composable
fun DragExample2() {
  var offset by remember { mutableStateOf(Offset.Zero) }
  var offsetX by remember { mutableStateOf(0f) }
  Box(
    modifier = Modifier
      .fillMaxSize()
  ) {
    Box(
      modifier = Modifier
        .fillMaxWidth()
        .height(20.dp)
        .offset { IntOffset(offsetX.roundToInt(), 0) }
        .background(Color.Blue)
        .pointerInput(Unit) {
          detectHorizontalDragGestures(
            onDragStart = { changeOffset ->
              offset = changeOffset
            },
            onDragEnd = {
              if(offsetX < -size.width || offsetX > size.width) {
                // 刪掉
              } else {
                // 彈回
                offsetX = 0f
              }
            },
            onDragCancel = {

            },
            onHorizontalDrag = { change, dragAmount ->
              offsetX += dragAmount
              change.consume()
            }
          )
        }
    ) {
      Text(" offsetX = ${offsetX.roundToInt()} x= ${offset.x} y = ${offset.y}")
    }
  }
}
{% endhighlight %}

## PointerInputScope.size
{% highlight kotlin linenos %}
interface PointerInputScope : Density {
    /**
     * The measured size of the pointer input region. 
     * Input events will be reported with a coordinate space of 
     * (0, 0) to (size.width, size.height) as the input region,
     * with (0, 0) indicating the upper left corner.
     */
    val size: IntSize
}
{% endhighlight %}