---
title: Column
date: 2023-05-03
keywords: Android, Jetpack compose, Column
---
Prerequisites:

- [row][1]
- [Scroll滾動Column][2]

Column的排列方式如下。<br>
![img]({{site.imgurl}}/compose/column1.png)<br>

Column的對齊都是控制Child子元件。

在英文文件，會提到Main axis主軸，也就是Column的排列方式是由上至下垂直排列。<br>

主軸，控制空格與位置，主軸的參數是Arrangement。

Cross-axis 交叉軸，就是跟Main axis主軸相反，就是水平位置，水平位置有:靠左、置中、靠右。<br>

交叉軸，控制位置，交叉軸的參數是，Alignment。

## 水平位置 horizontalAlignment
設定「子元件」(Child) 水平對齊方式。

### Alignment.CenterHorizontally 水平置中
![img]({{site.imgurl}}/compose/column_h_center.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testColumn() {
  Column(
    modifier = Modifier
      .height(300.dp)
      .width(200.dp),
    horizontalAlignment = Alignment.CenterHorizontally
  ) {
    Text("Item 1")
    Text("Item 2")
    Text("Item 3")
  }
}
{% endhighlight %}

### Alignment.Start 靠右
![img]({{site.imgurl}}/compose/column_h_start.png)<br>

### Alignment.End 靠左
![img]({{site.imgurl}}/compose/column_h_end.png)<br>

## 垂直對齊
設定「子元件」(Child) 垂直對齊方式。
### Arrangement.Center 垂直置中
![img]({{site.imgurl}}/compose/column_v_center.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testColumn() {
  Column(
    modifier = Modifier
      .height(300.dp)
      .width(200.dp),
    verticalArrangement = Arrangement.Center
  ) {
    Text("Item 1")
    Text("Item 2")
    Text("Item 3")
  }
}
{% endhighlight %}

### Arrangement.Top 靠上
![img]({{site.imgurl}}/compose/column_v_top.png)<br>

### Arrangement.Bottom 靠下
![img]({{site.imgurl}}/compose/column_v_bottom.png)<br>

### Arrangement.SpaceBetween
垂直平均分配，上下沒有空間。<br>
![img]({{site.imgurl}}/compose/column_v_between.png)<br>
### Arrangement.SpaceEvenly
垂直平均分配，上下有空間。<br>
![img]({{site.imgurl}}/compose/column_v_envenly.png)<br>

### Arrangement.SpaceAround
元素上下間距相等，上下有空間，item1與item2，有3格空間，item2與item3，有3格空間。<br>
```
·
·
[item1]
·
·
·
[item2]
·
·
·
[item3]
·
·
```
![img]({{site.imgurl}}/compose/column_v_around.png)<br>

-----------------------
以下是舊文章

{% highlight kotlin linenos %}
//SpaceEvenly每一個佈局間的間隔是一樣的
Column(
  modifier = Modifier
    .size(200.dp)
    .background(Color.Green),
  verticalArrangement = Arrangement.SpaceEvenly
){
//Modifier.weight(1f) 比重 fill預設為TRUE，填滿所有空間
  //先佈局沒有分配比重的Text()，剩餘的空間就分配給有比重
  Text(
    "Column First Item",
    modifier = Modifier.weight(1f, true)
  )
  Text("Column Second Item")
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/column2.png)  

{% highlight kotlin linenos %}
modifier = Modifier.weight(1f, false)   
{% endhighlight %}
![img]({{site.imgurl}}/compose/column3.png)

{% highlight kotlin linenos %}
//SpaceEvenly每一個佈局間的間隔是一樣的
Column(
  modifier = Modifier
    .size(200.dp)
    .background(Color.Green),
  verticalArrangement = Arrangement.SpaceEvenly
){
//Modifier.weight(1f) 比重 fill預設為TRUE，填滿所有空間
  //先佈局沒有分配比重的Text()，剩餘的空間就分配給有比重
  Text(
    "Column First Item",
    modifier = Modifier.weight(1f, true)
  )
  Text("Column Second Item",
    modifier = Modifier.weight(1f, true))
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/column4.png)

增加spacer  
{% highlight kotlin linenos %}
Column(
  modifier = Modifier
    .size(200.dp)
    .background(Color.Green)
){
//Modifier.weight(1f) 比重 fill預設為TRUE，填滿所有空間
  //先佈局沒有分配比重的Text()，剩餘的空間就分配給有比重
  Text(
    "Column First Item",
    modifier = Modifier.background(Color.Red)
  )
  Spacer(modifier = Modifier.height(10.dp))
  Text("Column Second Item",
    modifier = Modifier.background(Color.Red))
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/column5.png)  

{% highlight kotlin linenos %}
//SpaceEvenly每一個佈局間的間隔是一樣的
Column(
  modifier = Modifier
    .size(200.dp)
    .background(Color.Green),
  verticalArrangement = Arrangement.SpaceEvenly
) {
  //Modifier.weight(1f) 比重 fill預設為TRUE，填滿所有空間
  //先佈局沒有分配比重的Text()，剩餘的空間就分配給有比重
  Text(
    "Column First Item",
    modifier = Modifier.weight(1f, false)
  )
  Text("Column Second Item")
}
{% endhighlight %} 
![img]({{site.imgurl}}/compose/compose_column1.png) 

[1]: {% link _pages/compose/row.md %}
[2]: {% link _pages/compose/scroll.md %}