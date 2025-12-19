---
title: scale 放大縮小
date: 2025-12-19
keywords: Android, Jetpack compose, scale
---
## 語法
```
Modifier.scale(浮點數.f)
Modifier.scale(1.5f)
```

### 放大縮小數值
長寬比(Aspect Ratio)，長與寬同時放大縮小。<br>
`>1.0`是放大，`<1.0`是縮小。<br>

- 1.0f: 無放大縮小
- 1.5: 放大1.5倍
- 0.5: 縮小50%，縮小一半
- 0.75: 縮小25%
- 2: 放大2倍

![img]({{site.imgurl}}/compose/modifier/scale1.png)<br>

{% highlight kotlin linenos %}
@Composable
fun Scale() {
  Column(modifier = Modifier.fillMaxWidth()){
    // 不變
    Box(modifier = Modifier
      .size(100.dp)
      .scale(1.0f)
      .background(Color.Blue)
    ) {
      Text("Scale 1.0", color = Color.White)
    }
    // 放大1.5倍
    Box(modifier = Modifier
      .size(100.dp)
      .scale(1.5f)
      .background(Color.Blue)
    ) {
      Text("Scale 1.5", color = Color.White)
    }
    // 縮小一半
    Box(modifier = Modifier
      .size(100.dp)
      .scale(0.5f)
      .background(Color.Blue)
    ) {
      Text("Scale 0.5", color = Color.White)
    }
    // 放大2倍
    Box(modifier = Modifier
      .size(100.dp)
      .scale(2.0f)
      .background(Color.Blue)
    ) {
      Text("Scale 2.0", color = Color.White)
    }
  }
}
{% endhighlight %}