---
title: clip裁切
date: 2023-05-03
keywords: Android, Jetpack compose, clip
---
## 背景裁切
語法
```
Modifier.background(背景顏色,形狀(半徑))
Modifier.background(Color.Yellow,RoundedCornerShape(20.dp))
```

以下只有背景圖片有變圓角，但文字沒有被裁切。<br>
![img]({{site.imgurl}}/compose/modifier/clip1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(Color.Yellow,RoundedCornerShape(20.dp))
  ) {
    Text("Background")
  }
}
{% endhighlight %}

## 內容裁切
語法
```
Modifier.clip(RoundedCornerShape(20.dp))
```
以下只有內容被裁切。<br>
![img]({{site.imgurl}}/compose/modifier/clip1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(Color.Yellow)
      .clip(RoundedCornerShape(20.dp))
  ) {
    Text("Background")
  }
}
{% endhighlight %}

## 背景與內容都裁切
只要把clip()移到background()之前，就會背景跟內容都一起裁切。<br>
![img]({{site.imgurl}}/compose/modifier/clip3.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(RoundedCornerShape(20.dp))
      .background(Color.Yellow)
  ) {
    Text("Background")
  }
}
{% endhighlight %}

## 形狀
- RoundedCornerShape 四邊圓角
- CircleShape 圓形


### RoundedCornerShape 四邊圓角
語法
```
RoundedCornerShape(半徑)
RoundedCornerShape(16.dp)
```
圓角半徑是dp為單位。<br>
![img]({{site.imgurl}}/compose/modifier/clip3.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(RoundedCornerShape(16.dp))
      .background(Color.Yellow)
  ) {
    Text("Background")
  }
}
{% endhighlight %}

可以設定四個圓角的半徑。<br>
![img]({{site.imgurl}}/compose/modifier/clip7.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(RoundedCornerShape(
        topStart = 8.dp,
        topEnd = 16.dp,
        bottomStart = 24.dp,
        bottomEnd = 32.dp
      ))
      .background(Color.Blue)
  ) {
    Text("Background")
  }
}
{% endhighlight %}

可以只要左上、右上有圓角。<br>

![img]({{site.imgurl}}/compose/modifier/clip8.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(RoundedCornerShape(
        topStart = 8.dp,
        topEnd = 16.dp
      ))
      .background(Color.Blue)
  ) {
    Text("Background")
  }
}
{% endhighlight %}

### CircleShape 圓形
不用給參數，後面也不用帶圓括號()。<br>
![img]({{site.imgurl}}/compose/modifier/clip4.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(CircleShape)
      .background(Color.Yellow)
  ) {
    Text("Background")
  }
}
{% endhighlight %}

### CutCornerShape
語法
```
CutCornerShape(浮點數.dp)
```
四邊不是圓角，而是斜角，呈現菱形狀。<br>
![img]({{site.imgurl}}/compose/modifier/clip5.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(CutCornerShape(16.dp))
      .background(Color.Blue)
  ) {
    Text("Background")
  }
}
{% endhighlight %}

### 長方形
語法
```
.clip(RectangleShape)
```
不用給參數，後面也不用帶圓括號()。<br>

![img]({{site.imgurl}}/compose/modifier/clip6.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(RectangleShape)
      .background(Color.Blue)
  ) {
    Text("Background")
  }
}

{% endhighlight %}

## CornerSize 圓角尺吋
CornerSize提供2種參數:
- 固定大小 16.dp
- 百分比 比例 0.5f = 50%  0.25f = 25%

```
CornerSize(0.5f)
```

不知為何，以下沒有變化，待查詢。
{% highlight kotlin linenos %}
@Composable
fun testBrush() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .clip(RoundedCornerShape(CornerSize(0.5f)))
      .background(Color.Blue)
  ) {
    Text("Background")
  }
}
{% endhighlight %}
----------------------
以下是舊的文章

Clip圓角的作用區域
{% highlight kotlin linenos %}
.clip(RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp))//頂端二邊有圓角
{% endhighlight %}
要放在
```
.fillMaxSize()與.background()之前
```
{% highlight kotlin linenos %}
Column(modifier = Modifier
    .clip(RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp))//頂端二邊有圓角
    .fillMaxSize() //填滿
    .background(Color.White)//白色
){
Text(text = "明細")
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/clip.png)   