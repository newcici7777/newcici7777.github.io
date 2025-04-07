---
title: View Model
date: 2023-06-01
keywords: Android, Jetpack compose, View Model
---
參考以下網址
[https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=zh-cn](https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=zh-cn)  

將lifecycle的包打入  
![img]({{site.imgurl}}/compose/view_model1.png)  
```
var lifecycle_version = "2.5.1"
// Lifecycles only (without ViewModel or LiveData)
implementation("androidx.lifecycle:lifecycle-runtime-ktx:$lifecycle_version")
// ViewModel utilities for Compose
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:$lifecycle_version")
```
dataClass
![img]({{site.imgurl}}/compose/view_model2.png)  
![img]({{site.imgurl}}/compose/view_model3.png)  
![img]({{site.imgurl}}/compose/view_model4.png)  
建view
![img]({{site.imgurl}}/compose/view_model5.png)  
![img]({{site.imgurl}}/compose/view_model6.png)  
![img]({{site.imgurl}}/compose/view_model7.png)  