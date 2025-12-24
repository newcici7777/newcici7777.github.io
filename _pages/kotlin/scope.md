---
title: Scope生命周期
date: 2025-09-22
keywords: kotlin, coroutine scope lifecycle
---

|Scope| 生命週期|  主要用途|  使用場景|
|:--------|:---------|:----------|:-----------|
|withContext| 區塊執行期間|  切換執行緒| 需要切換執行緒的操作|
|viewModelScope|  ViewModel 生命週期|  MVVM 架構| ViewModel 中的資料操作|
|rememberCoroutineScope|  Composable 生命週期| UI 互動| Compose 中的用戶操作|
|lifecycleScope|  Activity/Fragment 生命週期|  Android 傳統 UI| Activity/Fragment 中|
|GlobalScope| 應用程式App生命週期|  極少用| 全域單次任務（慎用）|
|CoroutineScope()|  手動管理|  協程管理|  自訂Dispatchers、自訂執行緒、子協程取消管理、Exception|
|coroutineScope { }|  函數執行期間|  順序或同時執行子任務| 順序或同時執行suspend 函數|
|runBlocking 阻塞執行|  測試/主程式|  JUnit| 測試、main 函數|










