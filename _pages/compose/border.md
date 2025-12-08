---
title: border
date: 2025-12-08
keywords: Android, Jetpack compose, border
---
## 語法
```
border(width = 邊框厚度, color = 顏色)
border(width = 2.dp, color = Color.Red)
```

### Box 與 border
![img]({{site.imgurl}}/compose/modifier/border1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBorder() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .border(width = 2.dp, color = Color.Red)
      .background(Color.Blue)
  ) {
    Text("Border")
  }
}
{% endhighlight %}

### Text 與 border
![img]({{site.imgurl}}/compose/modifier/border2.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBorder() {
  Text(
    text = "Border",
    modifier = Modifier
      .border(
        width = 1.dp, color = Color.Blue
      )
  )
}
{% endhighlight %}

### border 與 形狀
- RoundedCornerShape 四邊圓角
- CircleShape 圓形
- CutCornerShape 菱角
- RectangleShape 長方形

![img]({{site.imgurl}}/compose/modifier/border3.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBorder() {
  Box(
    modifier = Modifier
      .size(200.dp)
      .border(
        width = 4.dp,
        color = Color.Red,
        shape = RoundedCornerShape(16.dp)
      )
      .background(Color.Blue)
  ) {
    Text("Test Border", color = Color.White)
  }
}
{% endhighlight %}

### border 與 brush
Prerequisites:

- [brush][1]

border使用brush參數，要搭配width、shape參數。<br>
![img]({{site.imgurl}}/compose/modifier/border4.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBorder() {
  Box(
    modifier = Modifier
      .size(200.dp)
      .border(
        width = 4.dp,
        brush = Brush.linearGradient(
          colors = listOf(Color.Yellow, Color.Magenta)
        ),
        shape = RoundedCornerShape(16.dp)
      )
      .background(Color.Blue)
  ) {
    Text("Test Border", color = Color.White)
  }
}
{% endhighlight %}

### border 與 padding
加上padding，裡面的文字就會放進框線中。<br>
![img]({{site.imgurl}}/compose/modifier/border5.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBorder() {
  Box(
    modifier = Modifier
      .size(200.dp)
      .border(
        width = 4.dp,
        brush = Brush.linearGradient(
          colors = listOf(Color.Yellow, Color.Magenta)
        ),
        shape = RoundedCornerShape(16.dp)
      )
      .background(Color.Blue)
      .padding(16.dp)
  ) {
    Text("Test Border", color = Color.White)
  }
}
{% endhighlight %}


[1]: {% link _pages/compose/brush.md %}
