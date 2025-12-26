---
title: draggable delta
date: 2025-12-26
keywords: Android, Jetpack compose, draggable, delta
---
Prerequisites:

- [IntOffset][1]

draggable 中文為滑鼠拖曳。

語法
```
Modifier
.draggable(
  orientation = Orientation.Horizontal,
  state = dragstate
)
```

orientation有二個參數
```
Orientation.Horizontal 水平方向拖曳
Orientation.Vertical 垂直方向拖曳
```

## delta
state的參數為 rememberDraggableState，語法如下:
{% highlight kotlin linenos %}
  val dragstate = rememberDraggableState { delta ->
  }
{% endhighlight %}

delta是拖曳的差距。<br>

假設x原本是在100的位置，滑鼠拖曳移到150的位置<br>
```
delta = 新移動的x座標 - 舊的x座標
delta = 150 - 100
```
delta就會為50f，注意！delta是浮點數。<br>

水平拖拽： Orientation.Horizontal
```
delta > 0  // 向右移動
delta < 0  // 向左移動
```

垂直拖拽：Orientation.Vertical
```
delta > 0  // 向下移動  
delta < 0  // 向上移動
```

```
初始位置：方塊在 x=0
      □
      
用戶開始拖拽...
系統偵測到手指移動：

第一次回調：delta = 15.0
     □ → 移動15px → □
     0         15

第二次回調：delta = -5.0 (稍微往回拉)
     □ ← 移動5px ← □
    10         15

第三次回調：delta = 30.0
     □ → 移動30px → □
    10         40

最終位置：x = 40
```

以下範例，以水平移動為範例，所以只有座標x會改變，delta是浮點數，所以offsetX也要設為浮點數，才能跟delta浮點數進行加減運算。<br>
{% highlight kotlin linenos %}
var offsetX by remember { mutableStateOf(0f) }
{% endhighlight %}

IntOffset參數只能為整數，但offsetX為浮點數，使用.roundToInt()四捨五入。
```
IntOffset(Int整數, Int整數)
```

![img]({{site.imgurl}}/compose/modifier/drag1.png)<br>

![img]({{site.imgurl}}/compose/modifier/drag2.png)<br>

{% highlight kotlin linenos %}
@Composable
fun DraggableExample1() {
  var offsetX by remember { mutableStateOf(0f) }
  val dragstate = rememberDraggableState { delta ->
    // 舊的x位置
    val oldPositionX = offsetX
    // 新的x位置 - 舊的x位置 = delta
    println("delta = $delta")
    // offset 設為新的x座標，加上delta差距
    offsetX += delta
  }
  Column {
    Box(
      modifier = Modifier
        .size(100.dp)
        // offsetX是浮點數 使用roundToInt()四捨五入轉成整數
        .offset { IntOffset(offsetX.roundToInt(), 0) }
        // background() 一定要放在offset之後
        .background(Color.Blue)
        .draggable(
          // 水平拖曳
          orientation = Orientation.Vertical,
          state = dragstate
        )
    )
  }
}
{% endhighlight %}

[1]: {% link _pages/compose/intoffset.md %}