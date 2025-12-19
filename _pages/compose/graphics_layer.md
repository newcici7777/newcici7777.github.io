---
title: graphicsLayer()
date: 2025-12-19
keywords: Android, Jetpack compose, graphicsLayer
---
Prerequisites:

- [offset][1]
- [rotate][2]
- [scale][3]

## 注意順序
graphicsLayer在background之前。
```
Modifier
  .size(100.dp)
  .graphicsLayer {
    translationX = 20.dp.toPx()
    translationY = 30.dp.toPx()
  }
  .background(Color.Blue)
```
## translation 移動
- translationX 向X軸(右左)移動
- translationY 向Y軸(上下)移動

移動的數值，請參考[offset][1]<br>

|正負數|x|y|
|:---:|:---:|:---:|
|正|向右|向下|
|負|向左|向上|

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
預設原點是在垂直置中。
### X軸
- 0.0 左邊
- 0.5 中間
- 1.0 右邊

### Y軸
- 0.0 最上面，頂部
- 0.5 垂直置中
- 1.0 最下面，底部

### 語法
```
Modifier.graphicsLayer {
    transformOrigin = TransformOrigin(X軸浮點數f,Y軸浮點數f)
}

Modifier.graphicsLayer {
    transformOrigin = TransformOrigin(0.5f,0.5f)
}
```

### XY軸
- TransformOrigin(0.0f,0.0f) 原點在左上角
- TransformOrigin(1.0f,0.0f) 原點在右上角
- TransformOrigin(0.5f,0.5f) 原點垂直置中
- TransformOrigin(0.0f,1.0f) 原點在左下角
- TransformOrigin(1.0f,1.0f) 原點在右下角

### 程式碼
以下範例，原點設為最左上角，從左上角向右旋轉10度。<br>
![img]({{site.imgurl}}/compose/modifier/graphic_origin1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun graphicExample() {
  Box(
    modifier = Modifier
      .size(300.dp)
      .graphicsLayer {
        transformOrigin = TransformOrigin(0.0f,0.0f)
        rotationZ = 10f
      }
      .background(Color.Blue)
      .border(2.dp,Color.Red)
  ) {
    Text("Scale 1.0", color = Color.White)
  }
}
{% endhighlight %}

## 放大縮小
- scaleX 長度放大縮小 
- scaleY 高度放大縮小

設定的數值請參考[scale][3]

語法
```
Modifier.graphicsLayer {
    scaleX = 浮點數f
    scaleY = 浮點數f
}
Modifier.graphicsLayer {
    scaleX = 2.0f
    scaleY = 2.0f
}
````

![img]({{site.imgurl}}/compose/modifier/graphic_scale1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun graphicExample() {
  Column {
    // 原本大小
    Box(
      modifier = Modifier
        .size(100.dp)
        .background(Color.Blue)
        .border(2.dp,Color.Red)
    ) {
      Text("Scale", color = Color.White)
    }
    // X軸，長度變2倍大
    Box(
      modifier = Modifier
        .size(100.dp)
        .graphicsLayer {
          transformOrigin = TransformOrigin(0.0f, 0.0f)
          scaleX = 2.0f
        }
        .background(Color.Blue)
        .border(2.dp,Color.Red)
    ) {
      Text("ScaleX 2.0f", color = Color.White)
    }
    // Y軸，高度變2倍大
    Box(
      modifier = Modifier
        .size(100.dp)
        .graphicsLayer {
          transformOrigin = TransformOrigin(0.0f, 0.0f)
          scaleY = 2.0f
        }
        .background(Color.Blue)
        .border(2.dp,Color.Red)
    ) {
      Text("ScaleY 2.0f", color = Color.White)
    }
    // X軸，長度變2倍大
    // Y軸，高度變1.5倍大
    Box(
      modifier = Modifier
        .size(100.dp)
        .graphicsLayer {
          transformOrigin = TransformOrigin(0.0f, 0.0f)
          scaleX = 2.0f
          scaleY = 1.5f
        }
        .background(Color.Blue)
        .border(2.dp,Color.Red)
    ) {
      Text("ScaleX = 2.0f ScaleY=1.5f", color = Color.White)
    }
  }
}
{% endhighlight %}

## 移動 旋轉 放大 原點 透明度
graphics layer可以同時設定多個設定。<br>

![img]({{site.imgurl}}/compose/modifier/graphic_all.png)<br>
{% highlight kotlin linenos %}
@Preview(showBackground = true, widthDp = 300, heightDp = 400)
@Composable
fun GreetingPreview2() {
  graphicExample1()
}

@Composable
fun graphicExample1() {
  Column {
    Box(
      modifier = Modifier
        .size(100.dp)
        .graphicsLayer {
          transformOrigin = TransformOrigin(0.0f, 0.0f)
          translationX = 20.dp.toPx()
          translationY = 30.dp.toPx()
          rotationZ = 10f
          scaleX = 1.5f
          scaleY = 2.0f
          alpha = 0.5f
        }
        .background(Color.Blue)
        .border(2.dp, Color.Red)
    ) {
      Text("Scale", color = Color.White)
    }
  }
}
{% endhighlight %}

[1]: {% link _pages/compose/offset.md %}
[2]: {% link _pages/compose/rotate.md %}
[3]: {% link _pages/compose/scale.md %}
