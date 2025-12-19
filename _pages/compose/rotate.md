---
title: rotate
date: 2025-12-18
keywords: Android, Jetpack compose, rotate
---
## 語法
```
Modifier.rotate(浮點數.f)
Modifier.rotate(45.f)
```
參數是旋轉的角度。

### 旋轉角度
- 0f: 無旋轉
- 45f: 向右旋轉45度
- -45f: 向左旋轉45度
- 90f: 向右旋轉90度
- -90f: 向左旋轉90度
- 180f: 上下顛倒(upside down)
- 270f: 向右旋轉270度

負數是向左旋轉，正數是向右旋轉。<br>

#### 45f
![img]({{site.imgurl}}/compose/modifier/rotate1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun Rotate() {
  Box(modifier = Modifier
    .size(100.dp)
    .rotate(45f)
    .background(Color.Blue)
  ) {
    Text("Rotated", color = Color.White)
  }
}
{% endhighlight %}

#### -45f
![img]({{site.imgurl}}/compose/modifier/rotate2.png)<br>
{% highlight kotlin linenos %}
@Composable
fun Rotate() {
  Box(modifier = Modifier
    .size(100.dp)
    .rotate(-45f)
    .background(Color.Blue)
  ) {
    Text("Rotated", color = Color.White)
  }
}
{% endhighlight %}

#### 180f
![img]({{site.imgurl}}/compose/modifier/rotate3.png)<br>
{% highlight kotlin linenos %}
@Composable
fun Rotate() {
  Box(modifier = Modifier
    .size(100.dp)
    .rotate(180f)
    .background(Color.Blue)
  ) {
    Text("Rotated", color = Color.White)
  }
}
{% endhighlight %}

## rotate Icon
- [icon][1]

![img]({{site.imgurl}}/compose/modifier/rotate4.png)<br>

{% highlight kotlin linenos %}
@Composable
fun RotateIconTest() {
  Column {
    RotateIcon("up")
    RotateIcon("right")
    RotateIcon("down")
    RotateIcon("left")
  }
}

@Composable
fun RotateIcon(direction: String) {
  val rotationAngle = when(direction) {
    "up" -> 0f
    "right" -> 90f
    "down" -> 180f
    "left" -> 270f
    else -> 0f
  }
  Icon(
    imageVector = Icons.Default.ArrowUpward,
    contentDescription = null,
    modifier = Modifier
      .size(48.dp)
      .rotate(rotationAngle)
  )
}
{% endhighlight %}

[1]: {% link _pages/compose/icon.md %}