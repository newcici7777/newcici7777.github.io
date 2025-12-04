---
title: Modifier 修飾子
date: 2025-12-01
keywords: JetpackCompose, Modifier
---
## 預設
每一個元件都會有修飾子，以下二種方式都是預設修飾子。<br>
注意！代入的Modifier，前面的M為大寫。<br>
kotlin函式指定參數寫法:<br>
```
Text("Hello", 參數名 = 參數值)
Text("Hello", modifier = Modifier)
```

Java的方法，沒辦法指定參數名稱，呼叫的時候一定要按照參數順序呼叫，但Kotlin可以宣告參數名，不用按照順序呼叫方法。<br>
```
方法(參數1, 參數2, 參數3);
```
{% highlight kotlin linenos %}
  // 方法1: 沒有代入modifier = Modifier
  Text("Hello")
  // 方法2: 代入modifier = Modifier
  Text("Hello", modifier = Modifier)
{% endhighlight %}

## Modifier Chain 鏈式呼叫
無法個別指定，大小、顏色、padding(產生空間間距)。
{% highlight kotlin linenos %}
  Modifier.size(100.dp, 200.dp)
  Modifier.background(color = Color.Red)
  Modifier.padding(8.dp)
{% endhighlight %}

使用Chain 鏈式呼叫。<br>
{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Text(
    "Hello", modifier = Modifier
      .size(200.dp, 300.dp)
      .background(Color.Blue)
      .padding(8.dp)
  )
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/modifier1.png)<br>

## 寬高設定
### width() height()
width()是寬，height()是高度。<br>

建立寬100dp，高150dp的盒子。<br>
{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Box(
    modifier = Modifier
      .width(100.dp)
      .height(150.dp)
      .background(color = Color.Red)
  )
}
{% endhighlight %}

如果高度不設，會根據箱子裡面裝的元件，自動調整高度，同理，若只設定height，不設定width，會根據箱子裡面的元件，自動調整寬度。

{% highlight kotlin linenos %}
  Box(
    modifier = Modifier
      .width(100.dp)
      .background(color = Color.Red)
  ) {
    Text("Hello", modifier = Modifier.height(20.dp))
  }
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/width1.png)<br>

### size()
size也是調整寬高，代入參數名width寬，或參數名height高，就可以設定寬高。<br>
{% highlight kotlin linenos %}
  Box(
    modifier = Modifier
      .size(width = 100.dp, height = 150.dp)
      .background(color = Color.Red)
  )
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/size1.png)<br>

若只代入一個數值，沒有參數名，代表要建立一個正方形，寬高都是相同。<br>

{% highlight kotlin linenos %}
  Box(
    modifier = Modifier
      .size(100.dp)
      .background(color = Color.Red)
  ) 
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/size2.png)<br>

## background()
設定顏色
{% highlight kotlin linenos %}
Modifier.background(color = Color.Red)
{% endhighlight %}

## padding
空間，英文是Space，我自己是翻釋成空間，但也可以翻成空白，或間距，或空格。<br>

padding()是增加空間(Space)，參數名start 為左、end 為右、top 為上、bottom 為下。 <br>
值是dp為單位，1dp代表，增加1dp大小的空間(Space)。<br>
dp 代表不會因為手機尺寸不同，大小不同，會根據不同手機大小，自動計算對映pixel像素，展示不同手機都有對映的大小。<br>

以下的意思是，左邊增加10dp大小的空間(Space)，右邊增加20dp大小的空間(Space)，上面增加30dp大小的空間(Space)，下方增加40dp大小的空間(Space)。<br>
```
Modifier.padding(start = 10.dp, end = 20.dp, top = 30.dp, bottom = 40.dp)
```

未設定padding前，上下左右都沒有空間(Space)。<br>
![img]({{site.imgurl}}/compose/modifier/padding1.png)<br>

未設定Text padding程式碼
{% highlight kotlin linenos %}
@Preview
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

設定padding後，上下左右，都各自增加空間(Space)，左邊start增加10dp空間，右邊end增加20dp空間，上面top增加30dp空間，下面bottom增加40dp空間。<br>

![img]({{site.imgurl}}/compose/modifier/padding2.png)<br>

{% highlight kotlin linenos %}
@Preview
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
        .padding(start = 10.dp, end = 20.dp, top = 30.dp, bottom = 40.dp)
    )
  }
}
{% endhighlight %}

上下左右都是增加10dp空間(Space)。<br>
```
Modifier.padding(start = 10.dp, end = 10.dp, top = 10.dp,  bottom = 10.dp)。
```
等同

設定「一個10dp」，表示上下左右都是增加10dp空間(Space)。<br>
```
Modifier.padding(10.dp)。
```

特定方向設定<br>
Modifier.padding(top = 10.dp, bottom = 5.dp)，表示只有上方和下方有增加空間(Space)。<br>
Modifier.padding(start = 8.dp, end = 8.dp)，表示只有左側和右側有增加空間(Space)。 <br>

### 往內增加空間
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

## 順序
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

## 根據父元件 決定大小
### fillMaxSize()
等同constraint layout中，寬高都是 match parent。<br>
若沒有父元件包住，fillMaxSize()意思就是跟手機螢幕大小一樣。<br>

### fillMaxSize() 與 padding
先fillMaxSize()寬高跟螢幕相同，再padding(20.dp)，就是往內，上下左右增加20dp的空間，內容元件Text，與螢幕邊緣有20dp空間大小的距離。

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

### fillMaxWidth
寬度根據父元件的寬度，若沒有父元件，就是螢幕的寬度。<br>
高度根據內容物(content)的高度，自適應高度。<br>
下面的例子，Box包住Text，Text就是Box的子元件，也是內容物(content)。<br>

![img]({{site.imgurl}}/compose/modifier/fullmaxwidth.png)

{% highlight kotlin linenos %}
@Preview
@Composable
fun GreetingPreview() {
  Box(modifier =Modifier
    .fillMaxWidth()
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

### fillMaxHeight
高度根據父元件的高度，若沒有父元件，就是螢幕的高度。<br>
寬度根據內容物(content)的寬度，自適應寬度。<br>

## Modifier 設定前後順序

設定前後順序如下:
1. 大小
2. 水平置中、垂直置中、權重
3. 背景顏色
4. padding
5. 形狀
