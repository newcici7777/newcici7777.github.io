---
title: 動畫
date: 2025-12-17
keywords: Android, Jetpack compose, Compositon Local
---
Prerequisites:

- [mutablestate][1]

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

以下是官網的範例<br>

<https://developer.android.com/develop/ui/compose/animation/composables-modifiers#animatedvisibility><br>

animateContentSize()要放置在size大小相關的函式之前。
{% highlight kotlin linenos %}
var expanded by remember { mutableStateOf(false) }
Box(
    modifier = Modifier
        .background(colorBlue)
        .animateContentSize()
        .height(if (expanded) 400.dp else 200.dp)
        .fillMaxWidth()
        .clickable(
            interactionSource = remember { MutableInteractionSource() },
            indication = null
        ) {
            expanded = !expanded
        }

) {
}
{% endhighlight %}

[1]: {% link _pages/compose/mutablestate.md %}