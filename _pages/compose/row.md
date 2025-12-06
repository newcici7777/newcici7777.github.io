---
title: Row
date: 2023-05-03
keywords: Android, Jetpack compose, Row
---
Row的排列方式如下。<br>
![img]({{site.imgurl}}/compose/row2.png)<br>

在英文文件，會提到Main axis主軸，也就是Row的排列方式是由左至右水平排列。<br>
Cross-axis 交叉軸，就是跟Main axis主軸相反，就是垂直位置，垂直位置有:靠上Top、垂直置中CenterVertically、靠下Bottom。<br>

## 垂直位置
verticalAlignment 設定「子元件」(Child) 垂直對齊方式，不是設定本身垂直對齊。<br>

子元件垂直對齊方式 = 靠上對齊<br>
![img]({{site.imgurl}}/compose/row3.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testRow() {
  Row(
    modifier = Modifier
      .fillMaxWidth()
      .height(200.dp),
    verticalAlignment = Alignment.Top
  ) {
    Text("Item 1")
    Text("Item 2")
    Text("Item 3")
  }
}
{% endhighlight %}

子元件垂直對齊方式 = 靠上對齊<br>
![img]({{site.imgurl}}/compose/row4.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testRow() {
  Row(
    modifier = Modifier
      .fillMaxWidth()
      .height(200.dp),
    verticalAlignment = Alignment.CenterVertically
  ) {
    Text("Item 1")
    Text("Item 2")
    Text("Item 3")
  }
}
{% endhighlight %}

子元件垂直對齊方式 = 置底<br>
![img]({{site.imgurl}}/compose/row5.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testRow() {
  Row(
    modifier = Modifier
      .fillMaxWidth()
      .height(200.dp),
    verticalAlignment = Alignment.Bottom
  ) {
    Text("Item 1")
    Text("Item 2")
    Text("Item 3")
  }
}
{% endhighlight %}

## 水平位置
參數是Arrangement，不是Alignment。<br>

預設水平排列方式是靠左。<br>

![img]({{site.imgurl}}/compose/row_start.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testRow() {
  Row(
    modifier = Modifier
      .fillMaxWidth()
      .height(200.dp),
    horizontalArrangement = Arrangement.Start
  ) {
    Text("Item 1")
    Text("Item 2")
    Text("Item 3")
  }
}
{% endhighlight %}

水平排列方式是置中。<br>

![img]({{site.imgurl}}/compose/row_center.png)<br>
```
horizontalArrangement = Arrangement.Center
```

水平排列方式是靠右。<br>

![img]({{site.imgurl}}/compose/row_end.png)<br>
```
horizontalArrangement = Arrangement.End
```

SpaceBetween，水平排列方式是均勻分散，左右二邊沒空格。<br>

![img]({{site.imgurl}}/compose/row_between.png)<br>
```
horizontalArrangement = Arrangement.SpaceBetween
```

SpaceEvenly，水平排列方式是均勻分散，左右有空格。<br>
![img]({{site.imgurl}}/compose/row_evenly.png)<br>
```
horizontalArrangement = Arrangement.SpaceEvenly
```

SpaceAround，水平排列方式是元素周圍間距相等，item1與item2，item2與item3，二個間距是相同。<br>
```
··[item1]···[item2]···[item3]··
```

item1與item2是3個空格。<br>
item2與item3是3個空格。<br>

![img]({{site.imgurl}}/compose/row_around.png)<br>
```
horizontalArrangement = Arrangement.SpaceAround
```

水平排列方式比較:
```
Start:     [A][B][C]·········
Center:    ····[A][B][C]·····
End:       ········[A][B][C]
SpaceBetween: [A]····[B]····[C]
SpaceEvenly: ·[A]····[B]····[C]·
SpaceAround: ··[A]···[B]···[C]··
```

-------------------------------
以下是舊文章

{% highlight kotlin linenos %}
Row(
  modifier = Modifier
    .size(200.dp)
    .background(Color.Green)
){
  Text(
    "Column First Item",
    modifier = Modifier.weight(1f, true)
  )
  Text("Column Second Item",
    modifier = Modifier.weight(1f, true))
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/row1.png)   