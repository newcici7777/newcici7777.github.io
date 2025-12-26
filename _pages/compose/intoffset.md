---
title: IntOffset
date: 2025-12-26
keywords: Android, Jetpack compose, offset Lambda
---
Prerequisites:

- [offset][1]

以下語法是繪在畫布固定位置
```
Modifier.offset(x = 20.dp, y = 10.dp)
```

## offset Lambda
以下範例，每按一次按鈕，藍方塊就往右移100px，注意！background()一定要放在`offset{}`後面，才看得出效果。<br>

![img]({{site.imgurl}}/compose/modifier/intoffset1.png)<br>

![img]({{site.imgurl}}/compose/modifier/intoffset2.png)<br>

步驟1:先把要移動的X「或」Y座標給預設值，並儲存起來。<br>
{% highlight kotlin linenos %}
var offsetX by remember { mutableStateOf(0) }
{% endhighlight %}

步驟2:設定Offset的參數是Lambda花括號`{}`，Lambda區塊裡面是:
```
IntOffset(你儲存的x座標,你儲存的y座標)
```

IntOffset參數只能為整數
```
IntOffset(Int整數, Int整數)
```

若要x座標不動，設為0。<br>
```
IntOffset(0,你儲存的y座標)
```

若要y座標不動，設為0。<br>
```
IntOffset(你儲存的x座標,0)
```

語法如下:
{% highlight kotlin linenos %}
Modifier.offset { IntOffset(offsetX, 0)}
{% endhighlight %}

以下程式碼，當執行的時候，按下按鈕，offsetX會增加100px，Compose就會重繪`offset { IntOffset(100, 0)}`這個語法，offsetX就會代入按鈕按下，新的offsetX座標，就會使方塊產生往右移動效果，因為y座標為0，固定不變，所以方塊呈現水平移動。<br>

完整程式碼:
{% highlight kotlin linenos %}
@Composable
fun OffsetLambdaExample1() {
  var offsetX by remember { mutableStateOf(0) }
  Column {
    Button(onClick = {
      offsetX += 100
    }) {
      Text("Click")
    }
    Box(modifier = Modifier
      .size(100.dp)
      .offset { IntOffset(offsetX, 0)}
      // background() 一定要放在offset之後
      .background(Color.Blue)
    )
  }
}
{% endhighlight %}



[1]: {% link _pages/compose/offset.md %}