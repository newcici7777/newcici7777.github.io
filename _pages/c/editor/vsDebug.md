---
title: VS Debug
date: 2024-09-02
keywords: VS Debug
---

F9設置/取消斷點
F5/F10開始調試
shift+F5放棄調試
F10逐過程執行
F11逐語句執行(進入函式)




## 設置中斷點

點擊圖下紅點位置。

![img]({{site.imgurl}}/vsdebug/debug1.png)

## 執行斷點

![img]({{site.imgurl}}/vsdebug/debug2.png)


## F5 移到斷點

### 第一次迴圈

以下紅框的按鈕為跳到下一個斷點，但目前範例只有在迴圈中一個斷點，也就意味下一個斷點就是下一次迴圈。

左邊會有變數的變化，區域變數word，會根據每一次迴圈，值有所不同。

![img]({{site.imgurl}}/vsdebug/debug3.png)

### 第二次迴圈

紅框的按鈕為跳到下一個斷點，區域變數word的值已經變成C++。

![img]({{site.imgurl}}/vsdebug/debug4.png)

### 查詢變數的值

![img]({{site.imgurl}}/vsdebug/debug5.png)

### 全部執行完斷點

在偵錯主控台並不會邊執行邊輸出，必須等到全部執行斷點，結果才會印出來。

![img]({{site.imgurl}}/vsdebug/debug6.png)

## F11 移到下一行程式碼

![img]({{site.imgurl}}/vsdebug/debug7.png)

![img]({{site.imgurl}}/vsdebug/debug8.png)