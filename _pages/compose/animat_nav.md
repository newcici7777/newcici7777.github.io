---
title: AnimationNavgation
date: 2023-05-03
keywords: Android, Jetpack compose, AnimationNavgation
---
首先確認匯入的package是對的
{% highlight kotlin linenos %}
var navigation_animation_version = "0.31.3-beta"
implementation "com.google.accompanist:accompanist-navigation-animation:$navigation_animation_version"
{% endhighlight %}

匯入的composable是com.google.accompanist.navigation.animation

其它的AnimatedNavHost/rememberAnimatedNavController也要檢查import的路徑是對的
{% highlight kotlin linenos %}
import com.google.accompanist.navigation.animation.composable
import com.google.accompanist.navigation.animation.AnimatedNavHost
import com.google.accompanist.navigation.animation.rememberAnimatedNavController
{% endhighlight %}

NavHostApp()
{% highlight kotlin linenos %}
package com.example.project1.ui.components
import android.util.Log
import androidx.compose.animation.ExperimentalAnimationApi
import androidx.compose.runtime.Composable
import com.example.project1.ui.navigation.Destinations
import com.example.project1.ui.screens.ArticleDetailScreen
import com.google.accompanist.navigation.animation.composable
import com.google.accompanist.navigation.animation.AnimatedNavHost
import com.google.accompanist.navigation.animation.rememberAnimatedNavController
import com.example.project1.ui.screens.MainFrame
/**
 * 導航控制器
 */
@OptIn(ExperimentalAnimationApi::class)
@Composable
fun NavHostApp() {
  val navController = rememberAnimatedNavController()
  AnimatedNavHost(
    navController = navController,
    startDestination = Destinations.HomeFrame.route
  ) {
    //HomeFrame作為起始頁
    composable(Destinations.HomeFrame.route){
      Log.d("xxxx","here1")
      MainFrame(onNavigateToArticle = {
        Log.d("xxxx","here3")
        navController.navigate(Destinations.ArticleDetail.route)
      })
    }
    //文章詳細頁
    composable(Destinations.ArticleDetail.route) {
      ArticleDetailScreen()
    }
  }
}
{% endhighlight %}

其中startDestination指的是預設最開始的頁面
{% highlight kotlin linenos %}
AnimatedNavHost(
        navController = navController,
        startDestination = Destinations.HomeFrame.route
    ) 
{% endhighlight %}

建立HomeFrame是那個函式處理的
{% highlight kotlin linenos %}
composable(Destinations.HomeFrame.route){
     MainFrame(onNavigateToArticle = {
         navController.navigate(Destinations.ArticleDetail.route)
     })
 }
{% endhighlight %}

要在MainActivity呼叫
{% highlight kotlin linenos %}
NavHostApp()
{% endhighlight %}