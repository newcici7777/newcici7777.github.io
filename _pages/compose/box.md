---
title: Box
date: 2023-05-03
keywords: Android, Jetpack compose, Box
---
## 堆疊
![img]({{site.imgurl}}/compose/compose_box1.png)<br>

若沒任何組件，默認是堆疊佈局<br>

寫在後面會把前面給疊上<br>

box疊在起來<br>

{% highlight kotlin linenos %}
Box(){
    Box(modifier = Modifier.size(200.dp).background(Color.Red)) //前面
    Box(modifier = Modifier.size(100.dp).background(Color.Green)) //後面
}
{% endhighlight %}

## align() 對齊
align()是給Box中的「Child」使用，Box是父親，Box包著的元件，就是孩子Child。<br>

- Alignment.Center 垂直置中 \+ 水平置中 
- Alignment.CenterStart 垂直置中 \+ 靠右對齊
- Alignment.CenterEnd 垂直置中 \+ 靠左對齊
- Alignment.TopStart 對齊頂部 \+ 靠右對齊
- Alignment.TopCenter 對齊頂部 \+ 水平置中 
- Alignment.TopEnd 對齊頂部 \+ 靠左對齊
- Alignment.BottomStart 對齊底部 \+ 靠右對齊
- Alignment.BottomCenter 對齊底部 \+ 水平置中 
- Alignment.BottomEnd 對齊底部 \+ 靠左對齊

為什麼Start是右邊呢？因為有些語系，寫字起始方向是由左朝右(如阿拉伯文)，所以面對不同的語系，「start」這個單字就比較明確的知道，是右邊開始，還是左邊開始。<br>

![img]({{site.imgurl}}/compose/modifier/box_align1.png)<br>

水平垂直置中
{% highlight kotlin linenos %}
@Composable
fun testBox() {
  Box(modifier = Modifier
    .size(200.dp)
    .background(Color.Gray)
  ) {
    Text("Hello",
      modifier = Modifier.align(Alignment.Center))
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/box_align2.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testBox() {
  Box(modifier = Modifier
    .size(200.dp)
    .background(Color.Yellow)
  ) {
    Text("TopStart",
      modifier = Modifier.align(Alignment.TopStart))
    Text("Center",
      modifier = Modifier.align(Alignment.Center))
    Text("BottomEnd",
      modifier = Modifier.align(Alignment.BottomEnd))
  }
}
{% endhighlight %}

## contentAligment()
contentAligment() 設定Box的所有Child子元件的對齊方式。<br>

如果Box的Child元件，沒有使用align()修飾子，預設就會使用父元件Box的contentAligment()修飾子。<br>

Child元件有使用align()修飾子，就會用自己設定的align()修飾子。<br>

![img]({{site.imgurl}}/compose/modifier/box_align3.png)<br>

預設
{% highlight kotlin linenos %}
@Composable
fun testBox() {
  Box(modifier = Modifier
    .size(200.dp)
    .background(Color.Yellow),
    contentAlignment = Alignment.CenterStart
  ) {
    Text("Hello")
  }
}
{% endhighlight %}

預設 與 Child自訂<br>

![img]({{site.imgurl}}/compose/modifier/box_align4.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testBox() {
  Box(modifier = Modifier
    .size(200.dp)
    .background(Color.Yellow),
    contentAlignment = Alignment.CenterStart
  ) {
    Text("Hello")
    Text("BottomEnd",
      modifier = Modifier.align(Alignment.BottomEnd))
  }
}
{% endhighlight %}

## Box中的Box
align是設置Box中的Child元件，但如果Child是Box，原理跟前面Text子元件一樣，align設置的是Child元件(Child Box)在父元件(Parent Box)「中」的位置。<br>

![img]({{site.imgurl}}/compose/modifier/box_align5.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testBox() {
  Box(
    modifier = Modifier
      .size(200.dp)
      .background(Color.Yellow),
    contentAlignment = Alignment.CenterStart
  ) {
    // 使用預設對齊
    Box(
      modifier = Modifier
        .size(20.dp)
        .background(Color.Blue)
    )
    // 使用自訂對齊
    Box(
      modifier = Modifier
        .size(20.dp)
        .background(Color.Blue)
        .align(Alignment.BottomEnd)
    )
  }
}
{% endhighlight %}
------------------------------------
以下是舊文章

置中  
`.align(Alignment.Center)`
{% highlight kotlin linenos %}
Box() {
    Box(
        modifier = Modifier
            .size(200.dp)
            .background(Color.Red)
    )
    Box(modifier = Modifier
        .align(Alignment.Center)
        .size(100.dp)
        .background(Color.Green))
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/compose_box2.png)