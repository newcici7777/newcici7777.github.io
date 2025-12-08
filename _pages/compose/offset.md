---
title: offset
date: 2025-12-08
keywords: Android, Jetpack compose, offset
---
## 語法
原點是左上角，座標是(x = 0, y= 0)。<br>
使用螢幕作標系統，不是迪卡爾數學x、y座標，二者不同。<br>
單位是dp。<br>

|正負數|x|y|
|:---:|:---:|:---:|
|正|向右|向下|
|負|向左|向上|

```
Modifier.offset(x = 正數向右移動, y = 正數向下移動)
Modifier.offset(x = 20.dp, y = 10.dp)

Modifier.offset(x = (負數)向左移動, y = (負數)向上移動)
Modifier.offset(x = (-20).dp, y = (-10).dp)
```
負數要用圓括號包住。

以下的例子，Box仍是在原本位置，但Box裡面的子元素Text，x向右移動20dp，y向下移動10dp。<br>
offset，不會影嚮父元素的位置。<br>

x與y都為正數。<br>
![img]({{site.imgurl}}/compose/modifier/offset1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testOffset() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(Color.Blue)
      .offset(x = 20.dp, y = 10.dp)
  ) {
    Text("Offset Box")
  }
}
{% endhighlight %}

x與y都為負數。<br>
![img]({{site.imgurl}}/compose/modifier/offset2.png)<br>
{% highlight kotlin linenos %}
fun testOffset() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(Color.Blue)
      .offset(x = (-20).dp, y = (-10).dp)
  ) {
    Text("Offset Box")
  }
}
{% endhighlight %}

## offset 與 background 順序
先後順序不同，效果就會不同。<br>

offset先，background後。<br>
![img]({{site.imgurl}}/compose/modifier/offset3.png)<br>
可以發現會針對offset的區域塗滿顏色。<br>
{% highlight kotlin linenos %}
@Composable
fun testOffset() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .offset(x = 10.dp, y = 30.dp)
      .background(Color.Blue)
  ) {
    Text("Test")
  }
}
{% endhighlight %}

background先，offset後。<br>
![img]({{site.imgurl}}/compose/modifier/offset4.png)<br>
background會先把box塗滿顏色，不會管後面有沒有位移。<br>
{% highlight kotlin linenos %}
@Composable
fun testOffset() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(Color.Blue)
      .offset(x = 10.dp, y = 30.dp)
  ) {
    Text("Test")
  }
}
{% endhighlight %}

