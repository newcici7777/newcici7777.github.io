---
title: graphicsLayer()
date: 2025-12-19
keywords: Android, Jetpack compose, graphicsLayer
---
Prerequisites:

- [offset][1]
- [rotate][2]
- [scale][3]

## translation 平移
- translationX 向X軸平移
- translationY 向Y軸平移

### translation 參數px
下方的數值要轉成px
```
Modifier.graphicsLayer {
    translationX = 數值.dp.toPx()
    translationY = 數值.dp.toPx()    
}
Modifier.graphicsLayer {
    translationX = 20.dp.toPx()
    translationY = 30.dp.toPx()    
}
```

![img]({{site.imgurl}}/compose/modifier/graphic_translation1.png)<br>

{% highlight kotlin linenos %}
@Composable
fun TranslateExample() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .graphicsLayer {
        translationX = 20.dp.toPx()
        translationY = 30.dp.toPx()
      }
      .background(Color.Blue)
      .border(2.dp,Color.Red)
  ) {
    Text("Scale 1.0", color = Color.White)
  }
}
{% endhighlight %}

## rotation
- rotationX 繞 X 軸旋轉
- rotationY 繞 Y 軸旋轉
- rotationZ 正常平面旋轉

角度數值是浮點數，後面要加上f
```
Modifier.graphicsLayer {
    rotationX = 45f
}
```

rotationZ 平面旋轉角度:
- 0f: 無旋轉
- 45f: 向右旋轉45度
- -45f: 向左旋轉45度
- 90f: 向右旋轉90度
- -90f: 向左旋轉90度
- 180f: 上下顛倒(upside down)
- 270f: 向右旋轉270度

rotationX<br>
![img]({{site.imgurl}}/compose/modifier/graphic_rotation_x.png)<br>

rotationY<br>
![img]({{site.imgurl}}/compose/modifier/graphic_rotation_y.png)<br>

rotationZ<br>
![img]({{site.imgurl}}/compose/modifier/graphic_rotation_z.png)<br>

X軸旋轉
{% highlight kotlin linenos %}
@Composable
fun TranslateExample() {
  Box(
    modifier = Modifier
      .size(300.dp)
      .graphicsLayer {
        rotationX = 45f
      }
      .background(Color.Blue)
      .border(2.dp,Color.Red)
  ) {
    Text("Scale 1.0", color = Color.White)
  }
}
{% endhighlight %}

向左平面旋轉10度。<br>
![img]({{site.imgurl}}/compose/modifier/graphic_rotation_z1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun TranslateExample() {
  Box(
    modifier = Modifier
      .size(300.dp)
      .graphicsLayer {
        rotationZ = -10f
      }
      .background(Color.Blue)
      .border(2.dp,Color.Red)
  ) {
    Text("Scale 1.0", color = Color.White)
  }
}
{% endhighlight %}

## TransformOrigin 設定原點座標
預設原點是在中間。

[1]: {% link _pages/compose/offset.md %}
[2]: {% link _pages/compose/rotate.md %}
[3]: {% link _pages/compose/scale.md %}
