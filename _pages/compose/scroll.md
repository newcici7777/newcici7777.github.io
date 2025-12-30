---
title: Column Row scroll
date: 2025-12-30
keywords: Android, Jetpack compose, scrollable
---
Prerequisites:

- [repeat][1]
- [rememberCoroutineScope][2]

Column 與 Row 滾動語法
```
val scrollState = rememberScrollState()
// Column 垂直滑動
Modifier.verticalScroll(scrollState)
// Row 水平滑動
Modifier.horizontalScroll(scrollState)
```

Column 與 Row 都是預先把子元素載入，適合用在表單填寫或靜態的內容。<br>

LazyColumn 與 LazyRow，滑動到(在螢幕上)才載入子元素，之前滑過的(沒有在螢幕上)會被記憶體回收。<br>

## Column
一開始就已經先畫好1000個元素在畫布上。
{% highlight kotlin linenos %}
@Composable
fun ScrollExample1() {
  val scrollState = rememberScrollState()
  Column (modifier = Modifier
    .fillMaxSize()
    .verticalScroll(scrollState)
  ) {
    repeat(1000) {
      Text("Item $it")
    }
  }
}
{% endhighlight %}

## Row
一開始就已經先畫好1000個元素在畫布上。
{% highlight kotlin linenos %}
@Composable
fun ScrollExample2() {
  val scrollState = rememberScrollState()
  Row (modifier = Modifier
    .fillMaxSize()
    .horizontalScroll(scrollState)
  ) {
    repeat(1000) {
      Text("Item $it")
    }
  }
}
{% endhighlight %}

## scrollTo 滑動到頂部 底部 中間
Column與Row不支援特定位置，以下是錯誤寫法，scrollTo(參數是pixel像素)。<br>
{% highlight kotlin linenos %}
scrollState.scrollTo(900)  // 這是像素值，不是項目索引！
{% endhighlight %}

scrollTo是suspend function，需要在協程中執行，所以使用Compose專用的協程作用域rememberCoroutineScope()。<br>
{% highlight kotlin linenos %}
  val scope = rememberCoroutineScope()
  scope.launch { scrollState.scrollTo(scrollState.maxValue/2) }
{% endhighlight %}

位置有以下三種:<br>
- Top: 0
- Bottom: scrollState.maxValue
- Middle: scrollState.maxValue / 2

語法:
```
// 頂部Top
scrollState.scrollTo(0)
// 底部Bottom
scrollState.scrollTo(scrollState.maxValue)
// 中間Middle
scrollState.scrollTo(scrollState.maxValue/2)
```

scrollto可替換成animateScrollTo。
{% highlight kotlin linenos %}
scrollState.animateScrollTo(scrollState.maxValue)
{% endhighlight %}

![img]({{site.imgurl}}/compose/modifier/scrollto.png)<br>

完整程式碼
{% highlight kotlin linenos %}
@Composable
fun ScrollExample3() {
  val scrollState = rememberScrollState()
  val scope = rememberCoroutineScope()
  Column (modifier = Modifier
    .fillMaxSize()){
    Row (modifier = Modifier
      .fillMaxWidth()){
      Button(onClick = {
        scope.launch { scrollState.scrollTo(scrollState.maxValue) }
      }) {
        Text("Scroll to Bottom")
      }
      Button(onClick = {
        scope.launch { scrollState.scrollTo(scrollState.maxValue/2) }
      }) {
        Text("Scroll to Middle")
      }
      Button(onClick = {
        scope.launch { scrollState.scrollTo(0) }
      }) {
        Text("Scroll to Top")
      }
    }
    Column (modifier = Modifier
      .verticalScroll(scrollState)
    ) {
      repeat(1000) {
        Text("Item $it")
      }
    }
  }
}
{% endhighlight %}

[1]: {% link _pages/kotlin/when_if.md %}#repeat
[2]: {% link _pages/compose/rememberCoroutineScope.md %}