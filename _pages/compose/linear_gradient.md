---
title: 背景顏色漸變
date: 2023-05-26
keywords: Android, Jetpack compose, linearGradient
---
## 背景顏色漸變
{% highlight kotlin linenos %}
Brush.verticalGradient(
listOf(Color(0xFF149EE7), Color.White))
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
Column(
  modifier = Modifier
    .fillMaxSize() //鋪滿全屏
    .background(
      //背景漸變色從上到下垂直漸變 從藍變白
      Brush.verticalGradient(
listOf(Color(0xFF149EE7), Color.White))
    )
){}
{% endhighlight %}

## 右上往左下漸變
{% highlight kotlin linenos %}
  Brush.linearGradient(
    listOf(Color(0xffbb8378), Color.Transparent),
		  //x值為屏幕的寬度 y值為頂端0
    start = Offset(x = screenWidth, y = 0f), 
		  //x值為0 y值為屏幕的高度
    end = Offset(x = 0f, y = screenHeight) 
  )
{% endhighlight %}
完整程式碼
{% highlight kotlin linenos %}
  var screenWidth: Float //螢幕寬度
  var screenHeight: Float //螢幕高度
  with(LocalDensity.current) {
  screenWidth = LocalConfiguration.current.screenWidthDp.dp.toPx()
  screenHeight = LocalConfiguration.current.screenHeightDp.dp.toPx()
  }
  //右上往左下漸變
  Box(
  modifier = Modifier
  .fillMaxSize()
  .background(
  Brush.linearGradient(
    listOf(Color(0xffbb8378), Color.Transparent),
		  //x值為屏幕的寬度 y值為頂端0
    start = Offset(x = screenWidth, y = 0f), 
		  //x值為0 y值為屏幕的高度
    end = Offset(x = 0f, y = screenHeight) 
  )
  )
  )
{% endhighlight %}

## 左下往右上漸變
![img]({{site.imgurl}}/compose/linear_gradient1.png) 
{% highlight kotlin linenos %}
start = Offset(x = 0f, y = screenHeight), 
end = Offset(x = screenWidth, y = 0f) 
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
//左下往右上漸變
Box(
  modifier = Modifier
  .fillMaxSize()
  .background(
    Brush.linearGradient(
    listOf(Color(0xFF149EE7), Color.Transparent),
    start = Offset(x = 0f, y = screenHeight), 
    end = Offset(x = screenWidth, y = 0f) 
    )
  )
)
{% endhighlight %}
![img]({{site.imgurl}}/compose/linear_gradient2.png) 