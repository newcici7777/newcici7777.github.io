---
title: System UI
date: 2023-05-03
keywords: Android, Jetpack compose, System bar, System UI, status bar
---
## System UI
```
┌─────────────────────────────────┐
│ Top    (Status Bar)             │ 
├─────────────────────────────────┤
│                                 │
│                                 │
│          你的内容                │
│                                 │
│                                 │
├─────────────────────────────────┤
│ Bottom (Navigation Bar)         │ 
└─────────────────────────────────┘
```

- Status Bar 頂部System UI
- Navigation Bar 底部System UI
- System Bar 頂部System UI \+ 底部System UI

## Status Bar
以下測試不能使用@Preview，會看不出來效果，要用模擬機測試。

沒有加Status Bar之前:<br>
![img]({{site.imgurl}}/compose/modifier/statusbar1.png)<br>

{% highlight kotlin linenos %}
import androidx.compose.foundation.layout.WindowInsets
import androidx.compose.foundation.layout.asPaddingValues
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.statusBars

class MainActivity : ComponentActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    enableEdgeToEdge()
    setContent {
      testStatus()
    }
  }
}

@Composable
fun testStatus() {
  Box(
    modifier = Modifier
      .fillMaxSize()
      //.padding(WindowInsets.statusBars.asPaddingValues())
      .background(Color.Blue)
  ) {
    Text("Test", color = Color.White)
  }
}
{% endhighlight %}

### WindowInsets.statusBars().asPaddingValues()
加Status Bar之後:<br>
![img]({{site.imgurl}}/compose/modifier/statusbar2.png)<br>

{% highlight kotlin linenos %}
@Composable
fun testStatus() {
  Box(
    modifier = Modifier
      .fillMaxSize()
      .padding(WindowInsets.statusBars.asPaddingValues())
      .background(Color.Blue)
  ) {
    Text("Test", color = Color.White)
  }
}
{% endhighlight %}

### statusBarsPadding()
效果與前一個程式碼相同。<br>
{% highlight kotlin linenos %}
@Composable
fun testStatus() {
  Box(
    modifier = Modifier
      .fillMaxSize()
      .statusBarsPadding()
      .background(Color.Blue)
  ) {
    Text("Test", color = Color.White)
  }
}
{% endhighlight %}

## systemBarsPadding() vs statusBarsPadding()
Status Bar，只會增加頂部 System UI空間。<br>
System Bar，會增加頂部System UI空間，與底部System UI空間。<br>


Status Bar只有螢幕上方有增加空間。<br>
![img]({{site.imgurl}}/compose/modifier/statusbar3.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testStatus() {
  Box(
    modifier = Modifier
      .fillMaxWidth()
      .statusBarsPadding()
  ) {
    Box(modifier = Modifier
      .fillMaxSize()
      .background(Color.Green)
    )
  }
}
{% endhighlight %}

System Bar是上方與下方都有增加空間。<br>
![img]({{site.imgurl}}/compose/modifier/systembar1.png)<br>
{% highlight kotlin linenos %}
@Composable
fun testStatus() {
  Box(
    modifier = Modifier
      .fillMaxWidth()
      .systemBarsPadding()
  ) {
    Box(modifier = Modifier
      .fillMaxSize()
      .background(Color.Green)
    )
  }
}
{% endhighlight %}

## Navigation Bar 
底部System UI <br>
![img]({{site.imgurl}}/compose/modifier/navbar1.png)<br>

以下程式碼，只會在底部增加空間。
{% highlight kotlin linenos %}
@Composable
fun testStatus() {
  Box(
    modifier = Modifier
      .fillMaxWidth()
      .padding(WindowInsets.navigationBars.asPaddingValues())
  ) {
    Box(modifier = Modifier
      .fillMaxSize()
      .background(Color.Green)
    )
  }
}
{% endhighlight %}

## imePadding()
{% highlight kotlin linenos %}
@Composable
fun WithIMEPadding() {
  Column(
    modifier = Modifier
      .fillMaxSize()
      .imePadding()  // 關鍵在這行！
  ) {
    // 上方內容
    Box(
      modifier = Modifier
        .weight(1f)
        .fillMaxWidth()
        .background(Color.LightGray)
    ) {
      Text("其他內容", modifier = Modifier.align(Alignment.Center))
    }

    var text by remember { mutableStateOf("") }

    // 2. 將狀態傳遞給 TextField
    TextField(
      value = text,  // 使用狀態值
      onValueChange = { newText ->
        text = newText  // 更新狀態
      },
      label = { Text("輸入文字") },
      modifier = Modifier
        .fillMaxWidth()
        .padding(16.dp)
    )
  }
}
{% endhighlight %}
----------------------------
以下是舊的文章

為什麼system bar會變白色  
![img]({{site.imgurl}}/compose/top_bar.png) 

原因是因為在MainActivity的Surface使用到MaterialTheme的 background預設是白色

{% highlight kotlin linenos %}
//使用這個會讓buttonNavigation被遮住
WindowCompat.setDecorFitsSystemWindows(window,false)
  setContent {
    Project1Theme {
      Surface(
        modifier = Modifier.fillMaxSize(),
        color = MaterialTheme.colors.background
      ) {
        NavHostApp()
      }
    }
  }
{% endhighlight %}

MainActivity修改成  
{% highlight kotlin linenos %}
import androidx.compose.material.MaterialTheme
import androidx.compose.material.Surface
WindowCompat.setDecorFitsSystemWindows(window,false)
  setContent {
    Project1Theme {
      Surface(
        modifier = Modifier.fillMaxSize(),
        color = MaterialTheme.colors.primary
      ) {
        NavHostApp()
      }
    }
  }
{% endhighlight %}

Article詳細頁修改成使用系統的TopAppBar  
{% highlight kotlin linenos %}
package com.example.project1.ui.screens
import androidx.compose.foundation.layout.statusBarsPadding
import androidx.compose.material.Scaffold
import androidx.compose.material.Text
import androidx.compose.material.TopAppBar
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
@Composable
fun ArticleDetailScreen() {
  Scaffold(
    topBar = {
	  //系統的TopAppBar
      TopAppBar(
        title = { Text(text = "Detail") }
      )
    },
	//system status bar的高度
    modifier = Modifier.statusBarsPadding()
  ) {
    Text(text = "文章詳情")
  }
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/top_bar.png) 

法二  
MainActivity 仍可使用預設白色  
{% highlight kotlin linenos %}
//使用這個會讓buttonNavigation被遮住
WindowCompat.setDecorFitsSystemWindows(window,false)
  setContent {
    Project1Theme {
      Surface(
        modifier = Modifier.fillMaxSize(),
        color = MaterialTheme.colors.background
      ) {
        NavHostApp()
      }
    }
  }
{% endhighlight %}

直接在Scaffold中的modifier寫入background，記得順序要在statusBarPadding()前面  
{% highlight kotlin linenos %}
package com.example.project1.ui.screens
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.statusBarsPadding
import androidx.compose.material.MaterialTheme
import androidx.compose.material.Scaffold
import androidx.compose.material.Text
import androidx.compose.material.TopAppBar
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
@Composable
fun ArticleDetailScreen() {
  Scaffold(
    topBar = {
      TopAppBar(
        title = { Text(text = "Detail") },
      )
    },
    modifier = Modifier
      .background(MaterialTheme.colors.primary)
      .statusBarsPadding()
  ) {
    Text(text = "文章詳情")
  }
}
{% endhighlight %}