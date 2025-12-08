---
title: JetpackCompose 準備
date: 2025-12-01
keywords: JetpackCompose
---
## 安裝JetpackCompse
![img]({{site.imgurl}}/compose/install1.png)<br>

選擇Groovy <br>
![img]({{site.imgurl}}/compose/install2.png)<br>

alt + cmd + shift + r <br> 
![img]({{site.imgurl}}/compose/install3.png)<br>

成功會出現以下的圖片<br>
![img]({{site.imgurl}}/compose/install4.png)<br>

畫面顯示全部仰賴`@Preview`，使用@Preview可預覽畫面。<br>

@Preview的GreetingPreview()函式，是沒有參數的。<br>

`(showBackground = true)`預覽畫面的背景有顏色，可以去掉。<br>
{% highlight kotlin linenos %}
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
  ComposeDemoTheme {
    Greeting("Android")
  }
}
{% endhighlight %}


testUI()

若有錯誤，請至下方錯誤排除。

## 錯誤排除
### 更新gradle
如果有出現以下這個錯誤
```
1.  Dependency 'androidx.navigationevent:navigationevent-android:1.0.0' requires Android Gradle plugin 8.9.1 or higher.

      This build currently uses Android Gradle plugin 8.9.0.
```

Project下的build.gradle
{% highlight groovy linenos %}
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath("com.android.tools.build:gradle:8.9.1")
        classpath("org.jetbrains.kotlin:kotlin-gradle-plugin:1.9.0")
    }
}
plugins {
alias(libs.plugins.android.application) apply false
    alias(libs.plugins.kotlin.android) apply false
    alias(libs.plugins.kotlin.compose) apply false
}
{% endhighlight %}

### 更新版本
```
  1.  Dependency 'androidx.navigationevent:navigationevent-android:1.0.0' requires libraries and applications that
      depend on it to compile against version 36 or later of the
      Android APIs.

      :app is currently compiled against android-35.

      Recommended action: Update this project to use a newer compileSdk
      of at least 36, for example 36.
```

App下的build.gradle，compileSdk改成36
{% highlight groovy linenos %}
android {
    namespace 'com.example.composedemo'
    compileSdk 36

    .
    .
    .
}
{% endhighlight %}