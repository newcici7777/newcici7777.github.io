---
title: Column
date: 2023-05-03
keywords: Android, Jetpack compose, Column
---
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