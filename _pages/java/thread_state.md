---
title: 執行緒狀態
date: 2025-06-12
keywords: java, Thread State, Blocked
---
## 執行緒有七種狀態

NEW(建立), RUNNABLE(執行階段), BLOCKED(阻斷), WAITING(等待), TIMED_WAITING(長時間等待), TERMINATED(結束);

## Runnable
Runnable包含Ready(準備)、Running(執行)。

有可能執行緒執行到一半，cpu資源不足，被強迫讓出yeild cpu資源，就從Running -> Ready。

如果cpu又有資源，執行緒又從 Ready -> Running。

當執行緒呼叫start()方法，狀態從 New -> Ready -> Running。

如果執行緒從sleep()方法中醒來，狀態也會從 TIMED_WAITING -> Ready -> Running。

如果執行緒拿到同步鎖，脫離Blocked阻斷狀態，狀態也會從 BLOCKED -> Ready -> Running。

## sleep TimeWaiting
```
Thread.sleep()
```
當執行緒在休眠時，此時執行緒是TimeWaiting狀態。
{% highlight java linenos %}
class T1 extends Thread{
  @Override
  public void run() {
    for (int i = 0; i < 10; i++) {
      System.out.println(Thread.currentThread().getName() + " i = " + i);
      try {
        // 20毫秒休息一次
        sleep(20);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test5 {
  public static void main(String[] args) throws InterruptedException {
    T1 t = new T1();
    // 設定名字
    t.setName("thread1");
    System.out.println(t.getName() + " state = " + t.getState());
    // 啟動執行緒
    t.start();

    // 判斷執行緒是否結束了
    while (t.getState() != Thread.State.TERMINATED) {
      System.out.println(t.getName() + " state = " + t.getState());
      // 50毫秒休息一次
      Thread.sleep(50);
    }
    System.out.println(t.getName() + " state = " + t.getState());
  }
}
{% endhighlight %}
```
thread1 state = NEW
thread1 state = RUNNABLE
thread1 i = 0
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 1
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 2
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 3
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 state = RUNNABLE
thread1 i = 4
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 5
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 6
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 7
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 8
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 i = 9
thread1 state = TIMED_WAITING
thread1 state = TIMED_WAITING
thread1 state = TERMINATED
```

由執行結果可以發現，thread1從NEW -> RUNNABLE -> TIMED_WAITING -> TERMINATED的過程。

## synchronized Blocked
當執行緒在同步(synchronized)區塊或同步方法的「外面」，此時執行緒是阻斷狀態。

## join Waiting
- [join 插隊][1]

當有其它執行緒插隊，此時執行緒變成Waiting或TimeWaiting狀態，等待其它執行緒執行完畢，狀態才會變成RUNNABLE。

## wait Waiting
Thread.wait()

當執行緒在等待wait()，此時執行緒變成Waiting或TimeWaiting狀態。


[1]: {% link _pages/java/join.md %}

