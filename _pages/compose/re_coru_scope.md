---
title: rememberCoroutineScope
date: 2025-12-25
keywords: Android, Jetpack compose, rememberCoroutineScope
---
為什麼要設定作用域rememberCoroutineScope？因為有些操作是很耗時，需要協程launch()來幫忙，要呼叫協程之前，需要使用Compose的作用域rememberCoroutineScope()，才能呼叫協程launch()。<br>

rememberCoroutineScope()會處理Compose重繪與移除Compose Tree(組件樹)、取消子協程、子協程產生Exception的生命周期。<br>

```
rememberCoroutineScope 父
  |-- launch 協程1 兒子1
  |-- launch 協程2 兒子2
  |-- launch 協程3 兒子3
```

程式碼展示父子關係
{% highlight kotlin linenos %}
@Composable
fun AnimatableExample() {
  // 父scope
  val scope = rememberCoroutineScope()
  Box(modifier = Modifier
    .fillMaxSize()
    .background(Color.Yellow)
    .clickable {
      // 協程1 兒子1
      scope.launch {
      }
      // 協程2 兒子2
      scope.launch {
      }
      // 協程3 兒子3
      scope.launch {
      }
    }
  ) 
}
{% endhighlight %}