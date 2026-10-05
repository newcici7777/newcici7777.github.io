---
title: setState()
date: 2026-10-05
keywords: flutter, setState()
---

在 Flutter 中，呼叫 setState(() {}); 的意思是：通知 Flutter 框架「狀態已經改變了」，並觸發畫面重新渲染（Rebuild）。

以下是詳細解析：

1. 核心作用
觸發重建 (build)：當你在 StatefulWidget 的 State 類別中呼叫 setState() 時，Flutter 會知道這個元件的資料變了，並在下一個影格（frame）重新執行該元件的 build 方法。

即時更新 UI：這讓你的使用者介面（UI）能夠即時反應變數或資料的最新變化。

2. 關於傳入的空閉包 () {}
標準用法：通常我們會把「修改變數」的程式碼寫在括號內部，例如：
{% highlight dart linenos %}
setState(() {
  _counter++; // 在這裡修改狀態變數
});
{% endhighlight %}

空實作的意義 (setState(() {});)：如果大括號裡面是空的，通常代表該變數已經在其他地方被改變了，而你強制要求 Flutter 重新整理畫面。不過，把修改邏輯直接寫在 setState 的大括號內是更推薦的寫法，這樣能確保狀態修改與畫面刷新同步，維持程式碼的清晰度。

小提醒：setState() 只能在 StatefulWidget 的 State 子類別中使用。如果在 StatelessWidget 或其他地方，是無法直接呼叫這個方法的。