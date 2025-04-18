---
title: Main Thread
date: 2025-04-07
keywords: Android, handler, thread, Runnable
---
Prerequisites:
- [thread][1]

## Main Thread, UI Thread
在Android中，關於使用者介面UI的操作都要放在主要執行緒(UI thread或Main thread)，其它耗時的操作(耗時的運算,IO,網路連線,資料庫存取...等等)則放在背景執行緒(Background Thread)，所謂的背景執行緒(Background Thread)也就是主流程(UI thread)之外的其它流程，一個程序只有一個主要執行緒(UI thread或Main thread)，但有很多個背景執行緒(Background Thread)。

舉例來說，在主要執行緒(UI thread)發出網路連線請求，應用程式的UI就會被凍結，凍結超過5秒，就會顯示應用程式無回應 (ANR) 對話方塊，所以要把網路連線請求放在背景執行緒(background Thread)，由背景執行緒(background Thread)處理長時間執行的網路連線流程，主要執行緒(UI thread)繼續處理UI更新。

## Main Thread有那些?

### Android生命周期
onCreate(),onStart,onResume()...等等。

### 事件
onClick(),onItemClick()...等等，on開頭的方法都是Main Thread。

### UI
例:TextView.setText()，跟UI有關的都是Main Thread。

## UI的同步與非同步
### 同步
等待上一個動作完成，才可以處理下一個動作。  
需要等待回傳結果，例如金流處理，付款等相關流程，需要有一個進度bar讓ui暫時凍結。

### 非同步
不用等待上一個動作完成，可以同時做多個動作。  
不需要等待回傳結果，例如批次下載圖片，可以先用暫時的圖片取代還未下載完成的圖片，不需要把ui凍結。使用者仍可進行其它ui操作。

lance

[1]: {% link _pages/java/thread.md %}