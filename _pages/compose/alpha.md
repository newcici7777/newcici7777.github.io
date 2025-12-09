---
title: alpha
date: 2025-12-09
keywords: Android, Jetpack compose, alpha
---
## 透明度
- 0.0f是完全透明
- 1.0f是沒有透明度
- 0.5f是50%透明度
- 0.25f是75%透明度

## Box與 alpha
透明度影嚮的是Box的Children子元件。<br>
以下的程式碼，是讓Box的「子元件」有透明度。<br>
不是讓Box背景顏色有透明度，雖然alpha是定義在Parent父元件上。<br>

![img]({{site.imgurl}}/compose/modifier/alpha1.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testAlpha() {
  Box(modifier = Modifier
    .size(200.dp)
    .background(Color.Blue)
    .alpha(0.2f)
  ) {
    Text("Alpha Test")
  }
}
{% endhighlight %}

## background 與 alpha
以下程式碼，建立3個box，分別是完全沒有透明度、設定透明度、background使用copy透明度。<br>
可以發現，Box設定透明度，影嚮的是子元件的透明度，背景顏色不影嚮。<br>
background使用copy的方式把複製出一個透明度80%(0.2f)的顏色，只有背景顏色有變，內容的Child子元件顏色不影嚮。<br>
![img]({{site.imgurl}}/compose/modifier/alpha2.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testAlpha() {
  Row {
    Box(
      modifier = Modifier
        .size(50.dp)
        .background(Color.Blue)
    ) {
      Text("Test1")
    }
    Box(
      modifier = Modifier
        .size(50.dp)
        .background(Color.Blue)
        .alpha(0.2f)
    ) {
      Text("Test1")
    }
    Box(
      modifier = Modifier
        .size(50.dp)
        .background(Color.Blue.copy(alpha = 0.2f))
    ) {
      Text("Test1")
    }
  }
}
{% endhighlight %}
## Button與alpha
未加alpha前的程式碼<br>
![img]({{site.imgurl}}/compose/modifier/btn_alpha1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testAlpha() {
  Button(onClick = {},
    enabled = false) {
    Text("disable click button")
  }
}
{% endhighlight %}

加了alpha後，會影嚮Button的背景透明度顏色，跟文字透明度的顏色。<br>
![img]({{site.imgurl}}/compose/modifier/btn_alpha1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testAlpha() {
  Button(onClick = {},
    modifier = Modifier.alpha(0.5f),
    enabled = false) {
    Text("disable click button")
  }
}
{% endhighlight %}