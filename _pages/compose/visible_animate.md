---
title: 顯示與隱藏動畫
date: 2025-12-17
keywords: Android, Jetpack compose, animateContentSize, AnimatedVisibility
---
## animateContentSize()
animateContentSize會根據大小不同產生動畫。<br>
![img]({{site.imgurl}}/compose/modifier/contentsize1.png)<br>

![img]({{site.imgurl}}/compose/modifier/contentsize2.png)<br>

{% highlight kotlin linenos %}
@Composable
fun AnimateContentSizeExample() {
  var isExpanded by remember { mutableStateOf(false) }
  Box(
    modifier = Modifier
      .fillMaxWidth()
      .animateContentSize()
      .background(Color.Blue)
      .padding(16.dp)
      .clickable {
        isExpanded = !isExpanded
      }
) {
    Column {
      Text("click")
      if(isExpanded) {
        Text("hello \n" +
            "world \n" +
            "hi \n",
          modifier = Modifier.background(Color.White))
      }
    }
  }
}
{% endhighlight %}

animateContentSize()要放置在size大小相關的函式之前。
{% highlight kotlin linenos %}
var expanded by remember { mutableStateOf(false) }
Box(
    modifier = Modifier
        .background(colorBlue)
        .animateContentSize()
        .height(if (expanded) 400.dp else 200.dp)
        .fillMaxWidth()
        .clickable() {
            expanded = !expanded
        }
)
{% endhighlight %}

## AnimatedVisibility 顯示與消失動畫
官方文件有許多進入動畫與離開動畫的展示效果。<br>

- [官方文件AnimatedVisibility](https://developer.android.com/develop/ui/compose/animation/composables-modifiers?hl=zh-tw) 

語法
{% highlight kotlin linenos %}
// 顯示或隱藏
var isVisible by remember { mutableStateOf(false) }
AnimatedVisibility(
  // 顯示或隱藏
  visible = isVisible,
  // 進入動畫
  enter = fadeIn(),
  // 離開動畫
  exit = fadeOut()
) {
	// 要顯示與隱藏的東西
	// 要顯示與隱藏的東西
}
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
@Composable
fun AnimatedVisibilityExample() {
  var isVisible by remember { mutableStateOf(false) }
  Column {
    Button(onClick = {
      isVisible = !isVisible
    }) {
      Text("Click")
    }
    AnimatedVisibility(
      visible = isVisible,
      enter = fadeIn(),
      exit = fadeOut()
    ) {
      Box(
        modifier = Modifier
          .size(100.dp)
          .background(Color.Blue)
      ) {
        Text("Content")
      }
    }
  }
}
{% endhighlight %}