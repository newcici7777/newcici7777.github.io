---
title: Scaffold
date: 2025-12-31
keywords: Android, Jetpack compose, Scaffold
---
參考資料:<br>

- [官方Scaffold](https://developer.android.com/develop/ui/compose/components/scaffold?hl=zh-tw)

## import
```
import androidx.compose.material3.*
```

## 語法
在@Composable上面要加上以下語法
```
@OptIn(ExperimentalMaterial3Api::class)
```

{% highlight kotlin linenos %}
Scaffold(
    // 標題欄
    topBar = {},
    // 底部欄
    bottomBar = {},
    // 懸浮按鈕
    floatingActionButton = {},
    // 內容
    content = {}
)
{% endhighlight %}

因為content為最後一個參數，所以最後一個參數可以變成Lambda。
{% highlight kotlin linenos %}
Scaffold(
    // 標題欄
    topBar = {},
    // 底部欄
    bottomBar = {},
    // 懸浮按鈕
    floatingActionButton = {}
) { innerPadding ->
	// 內容
}
{% endhighlight %}

## content 一定要使用innerPadding參數
如果不使用innerPadding會顯示不出來內容
{% highlight kotlin linenos %}
// content 方法一
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ScaffoldExample1() {
  Scaffold(
    topBar = {
      TopAppBar(title = { Text("My App") })
    },
    content = { innerPadding ->
      Text("Content", modifier = Modifier.padding(innerPadding))
    }
  )
}

// content 方法二
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ScaffoldExample2() {
  Scaffold(
    topBar = {
      TopAppBar(title = { Text("My App") })
    }
  ) {innerPadding ->
    Text("Content", modifier = Modifier.padding(innerPadding))
  }
}
{% endhighlight %}

## Bottom App bar
![img]({{site.imgurl}}/compose/scaffold1.png)  <br>
{% highlight kotlin linenos %}
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ScaffoldExample2() {
  Scaffold(
    topBar = {
      TopAppBar(title = { Text("Top App bar") })
    },
    bottomBar = {
      BottomAppBar {
        Text("Buttom App bar")
      }
    }
  ) { innerPadding ->
    Text("Content", modifier = Modifier.padding(innerPadding))
  }
}
{% endhighlight %}