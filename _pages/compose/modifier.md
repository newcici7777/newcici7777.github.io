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

## 根據父元件 決定大小
### fillMaxSize()
等同constraint layout中，寬高都是 match parent。<br>
若沒有父元件包住，fillMaxSize()意思就是跟手機螢幕大小一樣。<br>

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
1. 大小 : size, width, hight,fillMaxSize, fillMaxWidth, fillMaxHeight
2. 水平置中、垂直置中、權重: align, weight
3. 背景顏色 : background, border, alpha
4. padding : padding, offset
5. 形狀 : clip
