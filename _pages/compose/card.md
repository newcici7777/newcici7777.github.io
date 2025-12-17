---
title: Card
date: 2025-12-09
keywords: Android, Jetpack compose, Card
---
## Card語法
Card是MaterialTheme中的UI元件。<br>

{% highlight kotlin linenos %}
@Composable
fun testCard1() {
  Card(
    modifier = Modifier
      .size(100.dp)  //設定大小
  ) {
    Text("Hello", modifier = Modifier.padding(16.dp))
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/card1.png)<br>

## Card Colors
使用以下語法設定顏色。<br>
```
CardDefaults.cardColors{}
```

參數如下
```
// 卡片背景顏色
containerColor = colorScheme.primary,
// 文字顏色
contentColor = colorScheme.onPrimary
```

{% highlight kotlin linenos %}
@Composable
fun testCard1() {
  Card(
    modifier = Modifier
      .size(100.dp),
    colors = CardDefaults.cardColors(
      containerColor = Color.Yellow,
    )
  ) {
    Text("Hello", modifier = Modifier.padding(8.dp))
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/card2.png)<br>

## Card Elevation
Elevation，中文翻譯為高度，在Compose中，就像陰影。<br>
以下的例子要增加Card的padding，才看的出來效果。<br>
{% highlight kotlin linenos %}
@Composable
fun testCard1() {
  Card(
    modifier = Modifier
      .size(100.dp)
      .padding(8.dp)
    ,
    elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
  ) {
    Text("Hello", modifier = Modifier.padding(16.dp))
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/card3.png)<br>

## Card 與 disable
要加上`onClick = {}`，與enabled = false，才能加上以下語句。
```
// disable背景顏色
disabledContainerColor = 自訂顏色,
// disable文字顏色
disabledContentColor = 自訂顏色
```

{% highlight kotlin linenos %}
@Composable
fun testCard() {
  val colorScheme = MaterialTheme.colorScheme
  Card(
    // 加上onClick
    onClick = {},
    colors = CardDefaults.cardColors(
      // 正常的背景顏色
      containerColor = colorScheme.primary,
      // 正常的文字顏色
      contentColor = colorScheme.onPrimary,
      // disable的背景顏色
      disabledContainerColor = colorScheme.surfaceVariant,
      // disable的文字顏色
      disabledContentColor = colorScheme.onSurface
    ),
    modifier = Modifier
      .size(100.dp)
      .padding(16.dp),
    // 加上enabled
    enabled = false
  ) {
    Text("Hello", modifier = Modifier.padding(16.dp))
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/card4.png)<br>