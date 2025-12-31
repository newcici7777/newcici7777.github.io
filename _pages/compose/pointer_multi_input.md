---
title: awaitPointerEventScope 二指縮放
date: 2025-12-31
keywords: Android, Jetpack compose, awaitPointerEventScope, pointerInput, event, position
---
使用模擬器，按下ctrl + 滑鼠左鍵，即可模擬二指縮放大小。<br>

在 Jetpack Compose 中，awaitPointerEventScope 是 pointerInput 用於處理手勢和手指頭的交互動作。<br>
awaitPointerEvent是一個suspend function，是協程函式，用於監控手勢與手指頭的交互動作。<br>

使用`while(true)` 持續監聽手指碰觸螢幕，當Compose Tree被記憶體回收，就會停止監控。<br>
{% highlight kotlin linenos %}
Modifier
.awaitPointerEventScope {
   while (true) {
   }
}
{% endhighlight %}

判斷幾個手指頭在碰觸螢幕。
```
event.changes.size
```

取得每一個手指頭碰觸螢幕的座標
```
手指1
event.changes[0].position
手指2
event.changes[1].position
```

![img]({{site.imgurl}}/compose/pointer_multi1.png)<br>

![img]({{site.imgurl}}/compose/pointer_multi2.png)<br>

放大縮小程式碼:
{% highlight kotlin linenos %}
@Composable
fun ZoomExample1() {
  // 預設1f是無放大縮小
  var scale by remember { mutableStateOf(1f) }
  // 存放手指1座標
  var offset1 by remember { mutableStateOf(Offset.Zero) }
  // 存放手指2座標
  var offset2 by remember { mutableStateOf(Offset.Zero) }
  // 追蹤上一次的兩指距離
  var lastDistance by remember { mutableFloatStateOf(0f) }
  Box(
    modifier = Modifier
      .fillMaxSize()
      .pointerInput(Unit) {
        awaitPointerEventScope {
          while (true) {
            // 判斷是否有手指碰觸螢幕
            val event = awaitPointerEvent()
            // debug
            println("size = ${event.changes.size}")
            // 有沒有2隻手指碰觸
            if (event.changes.size >= 2) {
              // 取得手指1座標
              offset1 = event.changes[0].position
              // 取得手指2座標
              offset2 = event.changes[1].position
              // 計算二個座標距離
              val x_dis = offset1.x - offset2.x
              val y_dis = offset1.y - offset2.y
              val cur_dis = sqrt(x_dis * x_dis + y_dis * y_dis)
              if (lastDistance > 0) {
                // 目前距離 除 原本距離
                val scaleChange = cur_dis / lastDistance
                // coerceIn(最小值，最大值)
                // 縮放最小值不得低於0.5f，最大值不得大於3f
                scale = (scale * scaleChange).coerceIn(0.5f,3f)
                // 指派目前的距離至 lastDistance
                lastDistance = cur_dis
              } else {
                lastDistance = cur_dis
              }
            } else {
              // 若變成1隻手指觸碰，把lastDistance恢復原狀
              lastDistance = 0f
            }
          }
        }
      }
  ) {
    Column {
      Text("offset1 x= ${offset1.x.toInt()} y = ${offset1.y.toInt()}")
      Text("offset2 x= ${offset2.x.toInt()} y = ${offset2.y.toInt()}")
      Box(modifier = Modifier
        .size(200.dp)
        // 放大縮小
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