---
title: system bar
date: 2023-05-03
keywords: Android, Jetpack compose, System bar
---
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