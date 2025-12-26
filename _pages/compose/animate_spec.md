---
title: Animation Spec 動畫
date: 2025-12-17
keywords: Android, Jetpack compose, Animation Spec
---
## tween 控制動畫時間
[tween介紹](https://developer.android.com/develop/ui/compose/animation/customize?hl=zh-tw#tween)
### durationMillis 動畫持續時間
語法
{% highlight kotlin linenos %}
// 基本用法
val tweenSpec = tween<Float>(
    durationMillis = 300,      // 動畫持續時間（毫秒）
    delayMillis = 0,           // 延遲開始時間
    easing = LinearEasing      // 動畫速度
)
{% endhighlight %}

tween程式碼
{% highlight kotlin linenos %}
@Composable
fun AnimateContentSizeExample() {
  var isExpanded by remember { mutableStateOf(false) }
  Box(
    modifier = Modifier
      .fillMaxWidth()
      .animateContentSize(
        // 動畫持續1秒
        animationSpec = tween(durationMillis = 1000)
      )
      .background(Color.Blue)
      .padding(16.dp)
      .clickable {
        isExpanded = !isExpanded
      }
  ) {
    Column {
      Text("click")
      if (isExpanded) {
        Text("hello \n" + "world \n" + "hi \n",
          modifier = Modifier.background(Color.White)
        )
      }
    }
  }
}
{% endhighlight %}

### easing 
進入畫面與離開畫面的動畫速度
- LinearEasing 線性（均速）
- FastOutSlowInEasing    // 最常用的緩動
- FastOutLinearInEasing  // 快速出，線性進
- LinearOutSlowInEasing  // 線性出，慢速進

速度比較
```
easing = LinearEasing  // 線性：均速運動
// 0% ______ 50% ______ 100% (速度一樣)

easing = FastOutSlowInEasing  // 快速出，慢速入（最自然）
// 0% ████___ 50% ___██ 100% (開始快，結束慢)

easing = LinearOutSlowInEasing  // 線性出，慢速入
// 0% ______ 50% ___██ 100% (結束時減速)

easing = FastOutLinearInEasing  // 快速出，線性入
// 0% ████___ 50% ______ 100% (開始時加速)
```

其它
```
// 彈性效果
CubicBezierEasing(0.68f, -0.55f, 0.265f, 1.55f)

// 回彈效果
EaseOutBounce
```

## Spring 彈簧
Spring為animationSpec的預設值。
[Spring 介紹](https://developer.android.com/develop/ui/compose/animation/customize?hl=zh-tw#spring)
### dampingRatio 彈簧彈性
愈小愈有彈性
```
Spring.DampingRatioHighBouncy     // 0.2f - 高彈性
Spring.DampingRatioMediumBouncy   // 0.5f - 中等彈性
Spring.DampingRatioLowBouncy      // 0.75f - 低彈性
Spring.DampingRatioNoBouncy       // 1.0f - 無彈性（臨界阻尼）
```
自訂彈性
{% highlight kotlin linenos %}
val customSpring = spring<Float>(
    dampingRatio = 0.3f,  // 越小越有彈性
    stiffness = Spring.StiffnessMedium
)
{% endhighlight %}
### stiffness 彈簧剛硬度
愈低愈柔軟，愈會回彈。
```
Spring.StiffnessHigh       // 10_000f - 高剛度（快速）
Spring.StiffnessMedium     // 1_500f  - 中等剛度
Spring.StiffnessLow        // 200f    - 低剛度（緩慢）
Spring.StiffnessVeryLow    // 50f     - 非常低
```
自訂回彈
{% highlight kotlin linenos %}
// 自定義
val softSpring = spring<Float>(
    dampingRatio = Spring.DampingRatioMediumBouncy,
    stiffness = 100f  // 非常柔軟
)
{% endhighlight %}

範例程式
{% highlight kotlin linenos %}
@Composable
fun AnimateContentSizeExample1() {
  var isExpanded by remember { mutableStateOf(false) }
  Box(
    modifier = Modifier
      .fillMaxWidth()
      .animateContentSize(
        // 加上animationSpec
        animationSpec = spring(
          dampingRatio = Spring.DampingRatioHighBouncy,
          stiffness = Spring.StiffnessLow
        )
      )
      .background(Color.Blue)
      .padding(16.dp)
      .clickable {
        isExpanded = !isExpanded
      }
  ) {
    Column {
      Text("click")
      if(isExpanded) {
        Text("hello \n" +"world \n" +"hi \n",
          modifier = Modifier.background(Color.White))
      }
    }
  }
}
{% endhighlight %}


