---
title: Pointer event
date: 2025-12-31
keywords: Android, Jetpack compose, awaitPointerEventScope, Pointer event, awaitFirstDown, awaitPointerEvent
---
## 取得所有手指事件
{% highlight kotlin linenos %}
val event = awaitPointerEvent()
{% endhighlight %}

|動態|	pressed|	previousPressed|	意義|
|:----|:------:|:-------:|:----------|
|按下|	true|	false|	手指剛接觸屏幕|
|按下並滑動|	true|	true|	手指一直按著|
|放開|	false|	true|	手指剛離開屏幕|
|已抬起|	false|	false|	手指已經離開|

```
change.id                     // 手指 ID
change.id.value              // Long 類型的手指 ID
change.pressed               // 是否按下 (Boolean)
change.previousPressed       // 上一次事件時是否按下

change.position              // 當前按下位置 (Offset)
change.previousPosition      // 上一次按下位置(Offset)

// 位置變化方法
change.positionChanged()     // 位置是否改變 (Boolean)
change.positionChange()      // 位置變化量 (Offset) = position 按下位置 - previousPosition 上一個按下位置
```

![img]({{site.imgurl}}/compose/awaitpointer.png)<br>

{% highlight kotlin linenos %}
@Composable
fun PointerEventExample() {
  var state by remember { mutableStateOf("") }
  var info by remember { mutableStateOf("") }
  Box(
    modifier = Modifier
      .fillMaxSize()
      .pointerInput(Unit) {
        awaitPointerEventScope {
          while (true) {
            val event = awaitPointerEvent()
            event.changes.forEach { change ->
              info = buildString {
                appendLine("id = ${change.id.value}")
                appendLine("postion = ${change.position}")
                appendLine("previousPosition =${change.previousPosition}")
                appendLine("手指一直按並滑動 = ${change.positionChange()}")
                appendLine("是否滑動 = ${change.positionChanged()}")
              }
              when {
                change.pressed && !change.previousPressed ->
                  state = "按下"
                !change.pressed && change.previousPressed ->
                  state = "放開"
                change.pressed && change.previousPressed ->
                  state = "手指一直按"
                else ->
                  state = "沒動作"
              }
            }
          }
        }
      }
  ) {
    Column {
      Text("state = $state")
      Text("info = $info")
    }
  }
}
{% endhighlight %}

## 只取得按下事件

|特性|	awaitFirstDown()|	awaitPointerEvent() |
|:----|:---------|:---------|
|等待類型|	只等待按下事件|	等待任何事件（按下、移動、抬起）|
|返回內容|	單一 PointerInputChange|	PointerEvent 包含所有觸摸點|
|篩選能力	|自動篩選按下事件|	需要手動判斷事件類型|
|使用場景|	檢測點擊開始|	需要處理所有事件|

![img]({{site.imgurl}}/compose/firstdown.png)<br>

{% highlight kotlin linenos %}
@Composable
fun PointerEventExample2() {
  var info by remember { mutableStateOf("") }
  Box(
    modifier = Modifier
      .fillMaxSize()
      .pointerInput(Unit) {
        awaitPointerEventScope {
          while (true) {
            val down = awaitFirstDown()
            info = buildString {
              appendLine("id = ${down.id}")
              appendLine("position = ${down.position}")
              appendLine("time = ${down.uptimeMillis}")
            }
          }
        }
      }
  ) {
    Column {
      Text("info = $info")
    }
  }
}
{% endhighlight %}

