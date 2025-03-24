---
title: First jni
date: 2025-02-05
keywords: java, ndk, android, jni
---
## JNI
JNI是指程式運行時Java程式碼可以使用C或C++的lib，也可以在C或C++的lib使用Java程式碼。

## 建立JNI
![img]({{site.imgurl}}/ndk/create_ndk1.png)
![img]({{site.imgurl}}/ndk/create_ndk2.png)
![img]({{site.imgurl}}/ndk/create_ndk3.png)
![img]({{site.imgurl}}/ndk/create_ndk4.png)
![img]({{site.imgurl}}/ndk/create_ndk5.png)
![img]({{site.imgurl}}/ndk/create_ndk6.png)

## 檢查SDK
NDK與CMake要有勾選
![img]({{site.imgurl}}/ndk/check_sdk.png)

## cmake
自己建立lib
![img]({{site.imgurl}}/ndk/cmake1.png)
匯入(find)其它lib,自己建立的lib與其它lib綁定，並產生鏈結檔
![img]({{site.imgurl}}/ndk/cmake2.png)

## activity
![img]({{site.imgurl}}/ndk/activity1.png)
![img]({{site.imgurl}}/ndk/activity2.png)
![img]({{site.imgurl}}/ndk/activity3.png)

## 增加jni方法
滑鼠對著函式名(如下圖是滑鼠對著stringFromJNI2函式名)，按alt + enter，也可以產生jni方法
![img]({{site.imgurl}}/ndk/create_jni1.png)
![img]({{site.imgurl}}/ndk/create_jni2.png)
![img]({{site.imgurl}}/ndk/create_jni3.png)

