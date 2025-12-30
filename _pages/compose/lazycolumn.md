---
title: LazyColumn
date: 2023-05-03
keywords: Android, Jetpack compose, LazyColumn
---
Prerequisites:

- [repeat][1]
- [Scroll][2]

LazyColumn特性：
- 只渲染可見項目：只會渲染當前螢幕可見的項目，離開螢幕的項目會被回收
- 高效能：適合大量數據（成千上萬個項目)
- 支援複雜佈局：自動處理滾動狀態、內存管理
- 滾動狀態:	內建 LazyListState

## import
```
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
```

## 語法
{% highlight kotlin linenos %}
  LazyColumn {
    items(1000) {
      Text("$it")
    }
  }
{% endhighlight %}

![img]({{site.imgurl}}/compose/lazy_column1.png)  <br>

## item 單筆資料
可由多個 item 單筆資料組成。<br>

![img]({{site.imgurl}}/compose/lazy_column2.png)  <br>

{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample2() {
  LazyColumn {
    item {
      Text("Header")
    }
    item {
      Text("Body")
    }
    item {
      Text("Footer")
    }
  }
}
{% endhighlight %}

## items 指定數量
用在LazyColumn中，跟repeat一樣，輸出資料，但repeat不能使用在LazyColumn。<br>

以下是錯誤的寫法:<br>
{% highlight kotlin linenos %}
  LazyColumn {
    repeat(1000) {
      Text("$it")
    }
  }
{% endhighlight %}

正確寫法1
{% highlight kotlin linenos %}
  LazyColumn {
    items(1000) {
      Text("$it")
    }
  }
{% endhighlight %}

正確寫法2，執行結果跟上面相同。
{% highlight kotlin linenos %}
  LazyColumn {
    items(1000) { index ->
      Text("$index")
    }
  }
{% endhighlight %}

## items 與 list
![img]({{site.imgurl}}/compose/lazy_column3.png)  <br>
{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample3() {
  val data = listOf("Apple", "Orange", "Banana")
  LazyColumn {
    items(data) { fruit ->
      Text("$fruit")
    }
  }
}
{% endhighlight %}

## itemsIndexed 有索引
![img]({{site.imgurl}}/compose/lazy_column4.png)  <br>
{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample4() {
  val data = listOf("Apple", "Orange", "Banana")
  LazyColumn {
    itemsIndexed(data) { index, fruit ->
      Text("insex = $index ,fruit = $fruit")
    }
  }
}
{% endhighlight %}

## 滾動
Prerequisites:

- [List][3]

### rememberLazyListState
滾動到某個位置，需要使用rememberLazyListState()來幫忙，並把rememberLazyListState代入到LazyColumn的state參數中。<br>
語法:<br>
```
val lazyState = rememberLazyListState()
LazyColumn (state = lazyState) {}
```
### scrollToItem 滾動到指定項目
scrollToItem()是suspend function，需要在協程的作用域(Coroutines Scope)才能呼叫，支援Compose的Coroutines Scope是 rememberCoroutineScope。

語法如下:
```
val scope = rememberCoroutineScope()
scope.launch {
  lazyState.scrollToItem(100)
}
```

scrollToItem可替換為有動畫的animateScrollToItem
```
  lazyState.animateScrollToItem(100)
```

![img]({{site.imgurl}}/compose/lazy_column5.png)  <br>

完整程式碼:
{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample5() {
  val lazyState = rememberLazyListState()
  val scope = rememberCoroutineScope()
  Column {
    Row {
      Button(onClick = {
        scope.launch {
          lazyState.scrollToItem(100)
        }
      }) { Text("Scroll to 100") }
    }
    LazyColumn (state = lazyState) {
      items(1000) {
        Text("item = $it")
      }
    }
  }
}
{% endhighlight %}

### 滾動到頂部 底部 中間
LazyColumn的Scroll沒有像Column的[rememberScrollState][2]有maxValue，要把list移到前面，透過list.size滾動到最底部。

![img]({{site.imgurl}}/compose/lazy_column5.png)  <br>

完整程式碼
{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample5() {
  val lazyState = rememberLazyListState()
  val scope = rememberCoroutineScope()
  val list = List(100) {
    "Item $it"
  }
  Column {
    Row {
      Button(onClick = {
        scope.launch {
          lazyState.animateScrollToItem(0)
        }
      }) { Text("Top") }
      Button(onClick = {
        scope.launch {
          lazyState.animateScrollToItem(list.size / 2)
        }
      }) { Text("Middle") }
      Button(onClick = {
        scope.launch {
          lazyState.animateScrollToItem(list.size)
        }
      }) { Text("Scroll to 100") }
    }
    LazyColumn (state = lazyState){
      items(list) { item ->
        Text("item = $item")
      }
    }
  }
}
{% endhighlight %}

------------------------------
以下是舊文章

{% highlight kotlin linenos %}
LazyColumn(horizontalAlignment = Alignment.CenterHorizontally) {
}
{% endhighlight %}
設置前  
![img]({{site.imgurl}}/compose/lazycolumn1.png)  

設置後  
![img]({{site.imgurl}}/compose/lazycolumn2.png)  


[1]: {% link _pages/kotlin/when_if.md %}#repeat
[2]: {% link _pages/compose/scroll.md %}
[3]: {% link _pages/kotlin/list.md %}#其它建立List的方式