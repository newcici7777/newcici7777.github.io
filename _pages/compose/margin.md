---
title: Margin-like 增加外部空間
date: 2025-12-09
keywords: Android, Jetpack compose, padding, Margin-like
---
padding 通常是用來「內部」增加空間，但Card跟Button，使用padding是「外部」增空間，是margin的效果。<br>
Compose沒有margin，使用Margin-like，假裝是margin。<br>

## Card 與 padding
### Column 與 Card padding
以下程式碼，在Column父元件，包住三個Card子元件。<br>
每一個Card，定義padding bottom，向「外部底部」增加8dp的空間大小，注意！這邊不是內部。<br>

![img]({{site.imgurl}}/compose/modifier/card_margin1.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testCard() {
  Column (modifier = Modifier.fillMaxWidth()) {
    Card(
      modifier = Modifier
        .fillMaxWidth()
        .padding(bottom = 8.dp)
    ) {
      Text("Hello1")
    }

    Card(
      modifier = Modifier
        .fillMaxWidth()
        .padding(bottom = 8.dp)
    ) {
      Text("Hello2")
    }

    Card(
      modifier = Modifier
        .fillMaxWidth()
        .padding(bottom = 8.dp)
    ) {
      Text("Hello3")
    }
  }
}
{% endhighlight %}

### Row與 Card padding
以下程式碼，在Row父元件，包住二個Card子元件。<br>
每一個Card，定義padding horizontal，向「外部左邊與右邊」增加8dp的空間大小，注意！這邊不是內部。<br>

![img]({{site.imgurl}}/compose/modifier/card_margin1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testCard() {
  Row(
    modifier = Modifier
      .fillMaxWidth()
  ) {
    Card(
      modifier = Modifier
        .weight(1f)
        .padding(horizontal = 8.dp)
    ) {
      Text("Hello1")
    }

    Card(
      modifier = Modifier
        .weight(1f)
        .padding(horizontal = 8.dp)
    ) {
      Text("Hello2")
    }
  }
}
{% endhighlight %}

## Button與padding
以下尚未有padding，Button是貼近父元件邊緣。<br>

![img]({{site.imgurl}}/compose/modifier/button_padding1.png)<br>

showBackground = true ，要把背景顏色打開，才看的出效果。<br>
{% highlight kotlin linenos %}
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
  Button(
    onClick = {}
  ) {
    Text(text = "Hello !")
  }
}
{% endhighlight %}

加了top padding，與父元件有上方top距離20dp。<br>

![img]({{site.imgurl}}/compose/modifier/button_padding2.png)<br>

{% highlight kotlin linenos %}
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
  Button(
    onClick = {},
    modifier = Modifier
      .padding(top = 20.dp)
  ) {
    Text(text = "Hello !")
  }
}
{% endhighlight %}

### Button內部的元件 padding
Button內部的Text，Text的padding是屬於內增空間。

下面的例子，Text加上top padding。

![img]({{site.imgurl}}/compose/modifier/button_padding3.png)<br>

Text元件增加padding是「內部」增加上方20dp的空間，跟Button不一樣，Button是「外部」增加空間。

![img]({{site.imgurl}}/compose/modifier/button_padding3_.png)<br>

{% highlight kotlin linenos %}
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
  Button(
    onClick = {},
    modifier = Modifier.padding(top = 20.dp)
  ) {
    Text(text = "Hello !",
      modifier = Modifier.padding(top = 20.dp))
  }
}
{% endhighlight %}