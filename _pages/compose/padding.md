---
title: padding
date: 2023-05-03
keywords: Android, Jetpack compose, padding
---
空間，英文是Space，我自己是翻釋成空間，但也可以翻成空白，或間距，或空格。<br>

padding()是增加內部空間(Create internal space)，參數名start 為左、end 為右、top 為上、bottom 為下。 <br>

若不使用padding，子元件將會貼著父元件的邊緣，沒有距離。<br>

如下，Box包著Text，若沒有padding，Text就會貼近Box的邊緣。<br>

![img]({{site.imgurl}}/compose/modifier/padding1.png)<br>

{% highlight kotlin linenos %}
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(color = Color.Blue)
  ) {
    Text(
      text = "Hello !",
      modifier = Modifier
        .background(color = Color.Red)
    )
  }
{% endhighlight %}

下面設了padding，Text就不會緊貼著Box邊緣，而是與Box有左邊10dp、上面15dp的距離。

![img]({{site.imgurl}}/compose/modifier/padding2.png)<br>

{% highlight kotlin linenos %}
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(color = Color.Blue)
      .padding(start = 10.dp, top = 15.dp)
  ) {
    Text(
      text = "Hello !",
      modifier = Modifier
        .background(color = Color.Red)
    )
  }
{% endhighlight %}

語法
```
Modifier.padding(start = 10.dp, end = 20.dp, top = 30.dp, bottom = 40.dp)
```

意思是，向內部左邊增加10dp大小的空間，向內部右邊增加20dp大小的空間(Space)，向內部上面增加30dp大小的空間，向內部下方增加40dp大小的空間(Space)。<br>

### dp
dp是簡寫，全部的英文是 density-independent pixel

1dp = 160 DPI螢幕 的 1 pixel像素。

原文
```
1 density-independent pixel = one pixel on a 160 DPI screen
```

1dp代表，增加1dp大小的空間(Space)。<br>

dp 會根據不同手機大小，自動計算對映pixel像素，展示對映的大小。<br>

### padding(數字.dp)
以下第一行程式碼，上下左右增加10dp空間(Space)。<br>
以下第二行程式碼，設定「一個10dp」，表示上下左右都是增加10dp空間(Space)。<br>
二者都是相同意思。<br>
```
Modifier.padding(start = 10.dp, end = 10.dp, top = 10.dp,  bottom = 10.dp)
Modifier.padding(10.dp)
```

### 往內增加空間 Create internal space
下面的程式碼，Box包著Text，Box的寬高都是100dp，是一個正方形，顏色是藍色，從box的邊緣(edge)，向內部增加10dp的空間。<br>

也就是子元件Text與Box的邊緣，上面有10dp的距離，左邊也有10dp的距離，不會完全貼合box的邊緣。<br>

![img]({{site.imgurl}}/compose/modifier/padding_inside.png)<br>

![img]({{site.imgurl}}/compose/modifier/padding_inside1.png)<br>

{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Box(modifier =Modifier
    .size(100.dp)
    .background(color = Color.Blue)
    .padding(10.dp)
  ) {
    Text(
      text = "Hello !",
      modifier = Modifier
        .background(Color.Red)
    )
  }
}
{% endhighlight %}

### Button與padding
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

#### Button內部的元件 padding
Button內部的Text，也加上top padding。

![img]({{site.imgurl}}/compose/modifier/button_padding3.png)<br>

Text元件增加padding是增加上方20dp的「大小」，跟Button不一樣，Button是增加「距離」。

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

### Text padding
只單純設定Text元件，發現它會增加上方的空間。<br>
與Button不同，Button與父元件產生「距離」，而不是增加大小。<br>
![img]({{site.imgurl}}/compose/modifier/text_padding1.png)<br>
{% highlight kotlin linenos %}
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    Text(
      "Hello", modifier = Modifier
        .background(Color.Blue)
        .padding(top = 20.dp)
    )
}
{% endhighlight %}

#### Box內的Text padding
未設定padding前，Text 上下左右都沒有空間(Space)。<br>

![img]({{site.imgurl}}/compose/modifier/text_padding2.png)<br>

未設定Text padding程式碼
{% highlight kotlin linenos %}
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
  Box(
    modifier = Modifier
      .size(100.dp, 150.dp)
      .background(color = Color.Red)
  ) {
    Text(
      "Hello", modifier = Modifier
        .background(Color.Blue)
    )
  }
}
{% endhighlight %}

設定padding後，top = 20，Text元件，上方增加20dp空間(Space)。<br>

![img]({{site.imgurl}}/compose/modifier/text_padding3.png)<br>

{% highlight kotlin linenos %}
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
  Box(
    modifier = Modifier
      .size(100.dp, 150.dp)
      .background(color = Color.Red)
  ) {
    Text(
      "Hello", modifier = Modifier
        .background(Color.Blue)
        .padding(top = 20.dp)
    )
  }
}
{% endhighlight %}

### padding horizontal vertical
#### horizontal
向左側和右側有增加空間(Space)，以下二者寫法執行結果一模一樣。
```
Modifier.padding(horizontal = 8.dp)
Modifier.padding(start = 8.dp, end = 8.dp)
```
#### vertical
向上下增加空間。<br>
以下二者寫法執行結果一模一樣，但要根據元件判斷是增加大小、還是產生距離。
```
Modifier.padding(vertical = 10.dp)
Modifier.padding(top = 10.dp, bottom = 10.dp)
```

以下的程式碼，Box加上padding，內部增加左右10dp的空間，內部增加上下20dp的空間。

![img]({{site.imgurl}}/compose/modifier/padding_v_h.png)

![img]({{site.imgurl}}/compose/modifier/padding_v_h_.png)

{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(color = Color.Blue)
      .padding(
        horizontal = 10.dp,
        vertical = 20.dp
      )
  ) {
    Text(
      text = "Hello !",
      modifier = Modifier
        .background(color = Color.Red)
    )
  }
}
{% endhighlight %}

### padding 與 background 順序
#### padding -> background 
以下著重在Text()的Modifier，若padding()先，background()後面，Compose會先增加上下左右8dp大小的空間，排除掉增加的上下左右空間，然後才增加背景顏色。<br>
{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Box(modifier =Modifier
    .size(200.dp,300.dp)
    .background(color = Color.Blue)
  ) {
    Text(
      text = "Hello !",
      modifier = Modifier
        .padding(8.dp)
        .background(Color.Red)
    )
  }
}
{% endhighlight %}

Text未設置Modifier前。<br>
![img]({{site.imgurl}}/compose/modifier/sequence1.png)<br>

Text設置Modifier，padding()先，background()後面。<br>
![img]({{site.imgurl}}/compose/modifier/sequence2.png)<br>

#### background -> padding 
background()先，padding()後，就會把增加的空間，也塗上背景顏色。<br>
{% highlight kotlin linenos %}
Text(
  text = "Hello !",
  modifier = Modifier
    .background(Color.Red)
    .padding(8.dp)    
)
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/sequence3.png)<br>

#### background -> padding -> background
以下的程式碼，順序如下:
1. 設定大小為寬高100dp的正方形
2. 設定正方形的背景顏色(藍色)
3. 往內增加上下左右10dp空間
4. 排除掉增加的上下左右10空間，內部增加背景顏色(紅色)

![img]({{site.imgurl}}/compose/modifier/sequence4.png)<br>

{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Box(modifier =Modifier
    .size(100.dp)
    .background(color = Color.Blue)
    .padding(10.dp)
    .background(color = Color.Red)
  ) {
    Text(text = "Hello !")
  }
}
{% endhighlight %}

### fillMaxSize() 與 padding
先fillMaxSize()寬高跟螢幕相同，再padding(20.dp)，就是往內上下左右增加20dp的空間，內部元件Text與螢幕邊緣有20dp空間大小的距離。<br>
排除掉增加的上下左右20dp空間，內部增加背景顏色(紅色)。<br>

順序如下:
1. fillMaxSize()寬高跟螢幕相同
2. 往內增加上下左右20dp空間
3. 排除掉增加的上下左右20dp空間，內部增加背景顏色(紅色)

![img]({{site.imgurl}}/compose/modifier/fullmaxsize_padding.png)<br>

{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Box(modifier =Modifier
    .fillMaxSize()
    .padding(20.dp)
    .background(color = Color.Blue)
  ) {
    Text(
      text = "Hello !",
      modifier = Modifier
        .background(Color.Red)
        .padding(8.dp)
    )
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/fullmaxsize_padding.png)
