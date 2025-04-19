---
title: Callback模式
date: 2025-04-18
keywords: Design patten, Callback patten
---
## Callback回呼介面
Callback（回呼）是一種常見的設計模式，允許一個類別在特定事件發生時通知另一個類別。

建立Callback介面
{% highlight java linenos %}
public interface SimpleCallback {
  void onComplete(String result);
}
{% endhighlight %}

實作Callback介面
{% highlight java linenos %}
public class CallbackHandler implements SimpleCallback {
  @Override
  public void onComplete(String result) {
  System.out.println("Callback 收到結果: " + result);
  }
}
{% endhighlight %}

使用已實作的Callback介面
{% highlight java linenos %}
public class Task {
  public void executeTask(SimpleCallback callback) {
    System.out.println("任務執行中...");
    // 模擬長時間運算
    try {
      Thread.sleep(2000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    
    // 任務完成後呼叫 callback
    callback.onComplete("任務完成!");
  }
}
{% endhighlight %}

主程式
{% highlight java linenos %}
public class Main {
  public static void main(String[] args) {
    Task task = new Task();
    SimpleCallback callback = new CallbackHandler();
    
    System.out.println("開始執行任務...");
    task.executeTask(callback);
    System.out.println("任務已啟動 (非同步執行)");
  }
}
{% endhighlight %}