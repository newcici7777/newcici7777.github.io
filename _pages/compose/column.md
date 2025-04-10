---
title: Column
date: 2023-05-03
keywords: Android, Jetpack compose, Column
---
![img]({{site.imgurl}}/compose/column1.png)  

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