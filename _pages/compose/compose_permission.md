---
title: Permission
date: 2023-05-28
keywords: Android, Jetpack compose, Permission
---
Protection Leveld是危險才出現對話
![img]({{site.imgurl}}/compose/permission1.png)
在Manifest.permission.CAMERA前面要加上anroid.  
不然會CAMERA會出現紅色 reference不到  
如下圖  
anroid.Manifest.permission.CAMERA  
![img]({{site.imgurl}}/compose/permission2.png)
還要參考ADB設定  
官方說，要讓模擬器有permission的功能要用 -g  
先把apk匯出再透過下面指令安裝，以後就不用這樣了  
adb install -g app-debug.apk  
然後參考run Activity文件  
打開模擬器，想辦法把Setting弄出來(到主頁從下往上拉)  
選擇Pemissions  
![img]({{site.imgurl}}/compose/permission3.png)
選擇如下圖see all apps......
![img]({{site.imgurl}}/compose/permission4.png)
往下滑動，看到MyApplicationApp，點進去
![img]({{site.imgurl}}/compose/permission5.png)
選擇ask everytime，才方便測試跳出權限使用的對話框
![img]({{site.imgurl}}/compose/permission6.png)
跳出的對話框
![img]({{site.imgurl}}/compose/permission7.png)
第二種方法
![img]({{site.imgurl}}/compose/permission8.png)
![img]({{site.imgurl}}/compose/permission9.png)
點擊Camera
![img]({{site.imgurl}}/compose/permission10.png)
![img]({{site.imgurl}}/compose/permission11.png)
要全部重新來過Permission設定
可以刪除App重新來過
![img]({{site.imgurl}}/compose/permission12.png)
![img]({{site.imgurl}}/compose/permission13.png)