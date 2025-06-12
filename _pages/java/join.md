---
title: join 插隊
date: 2025-06-12
keywords: Android, java, synchronized, join
---
所謂的join是，當所有執行緒都在同時執行的時候，某個執行緒使用join占用cpu的資源，其它執行緒就休息，待它執行完畢，讓出cpu資源，其它執行緒再繼續執行。

{% highlight java linenos %}
class R3 implements Runnable{
  @Override
  public void run() {
    for (int i = 0; i < 10; i++) {
      System.out.println(Thread.currentThread().getName() + " i = " + i);
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }
  }
}
{% endhighlight %}

以下有main執行緒與執行緒1，當main執行緒執行到i \=\= 5，把CPU讓出來給執行緒1，等到執行緒1執行完，main再繼續執行。
{% highlight java linenos %}
public class Test4 {
  public static void main(String[] args) throws InterruptedException {
    R3 r3 = new R3();
    Thread t1 = new Thread(r3);
    t1.setName("執行緒1");
    // 啟動
    t1.start();

    for (int i = 0; i < 10; i++) {
      if (i == 5) {
        // 讓執行緒1插隊先執行，main執行緒休息
        t1.join();
        // 執行緒1執行完，main執行緒再接著執行
      }
      System.out.println(Thread.currentThread().getName() + " i = " + i);
      Thread.sleep(1000);
    }
  }
}
{% endhighlight %}
```
main i = 0
執行緒1 i = 0
main i = 1
執行緒1 i = 1
main i = 2
執行緒1 i = 2
執行緒1 i = 3
main i = 3
main i = 4
執行緒1 i = 4
執行緒1 i = 5
執行緒1 i = 6
執行緒1 i = 7
執行緒1 i = 8
執行緒1 i = 9
main i = 5
main i = 6
main i = 7
main i = 8
main i = 9
```

從執行結果可以發現，一開始都是同時執行的，到了main i = 4之後，main執行緒暫停，輪到執行緒1執行完，main執行緒才繼續執行。