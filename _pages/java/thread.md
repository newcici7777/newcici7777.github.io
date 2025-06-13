---
title: Thread
date: 2025-04-07
keywords: Android, java, thread, Runnable
---
Prerequisites:

- [匿名內部類別][1]
- [介面][2]

## 名詞介紹
程式(Program)透過javac編譯後，產生了.class檔，.class檔又稱為位元碼。
```
javac 類別.java
```

把.class位元碼載入到記憶體，程式執行的過程，稱之為程序(Process)。
```
java 類別
```

可以執行多次`java 類別`指令，產生很多程序。

![img]({{site.imgurl}}/java/process1.png)

操作系統(OS)可以管理多個程序(Process)，可以讓多個程序(Process)輪流使用CPU資源。

Jvm視為虛擬的操作系統，管理多個程序中的多個執行緒。

每一個執行緒(Thread)都是一個流程。

每個程序(Process)都有一個主要執行緒(Main Thread)，也稱作主要流程，是程式的入口。

想「同時執行」很多流程，每一個流程都是一個執行緒，下圖中，main主要執行緒啟動了2個執行緒，其中一個執行緒，又再啟動2個執行緒，大家來「同時」執行。

![img]({{site.imgurl}}/java/thread1.png)

### Concurrent併發
一個CPU執行多個執行緒，Jvm有一個排程管理系統，會十分快速的將一個流程(執行緒)切換到另一個流程(執行緒)，快速的切換讓使用者以為是同時執行這些流程(執行緒)，實際上是十分快速的輪流執行(執行緒)。

![img]({{site.imgurl}}/java/cpu1.png)

### Parallel
多個CPU執行多個執行緒，平行執行任務。

![img]({{site.imgurl}}/java/cpu2.png)

但執行緒若有100個，不可能有100個CPU彼此對映，大部分的時候都是Concurrent \+ Parallel互相搭配，1核心就是1個CPU，8核心就是8個CPU，可能8個CPU分配處理100個執行緒，每一個CPU快速切換在各個不同執行序之間。

## Runnable 介面
要實作Runnable，並且覆寫run方法，類別才能變成執行緒。
{% highlight java linenos %}
public interface Runnable {
    void run();
}
{% endhighlight %}

## Thread原始碼
Thread實作Runnable介面，並且覆寫run()方法。

實作Runnable介面的類別，才可以放進Thread(Runnable task)建構子。
{% highlight java linenos %}
public class Thread implements Runnable {
    // 無參數建構子
    public Thread() {
        this(null, null, 0, null, 0, null);
    }

    // 參數是有實作Runnable的類別
    public Thread(Runnable task) {
        this(null, null, 0, task, 0, null);
    }

    // 覆寫run方法
    public void run() {
        Runnable task = holder.task;
        if (task != null) {
            Object bindings = scopedValueBindings();
            runWith(bindings, task);
        }
    }

    // 啟動執行緒
    public void start() {
        synchronized (this) {
            // zero status corresponds to state "NEW".
            if (holder.threadStatus != 0)
                throw new IllegalThreadStateException();
            // 呼叫Native C++的執行緒方法
            start0();
        }
    }

    // Native C++的執行緒方法，start0會呼叫run()方法
    private native void start0();
}
{% endhighlight %}

### Thread.start()
啟動執行緒。

### Thread.run()
不是啟動執行緒，run()是給native c++的start0()呼叫。

使用run()方法，就等同於main呼叫方法，並不是啟動執行緒。

### Thread.sleep()
讓執行緒暫時休息。

參數是毫秒，1秒是1000毫秒，0.5秒是500毫秒，0.1秒是100毫秒，0.01秒是10毫秒，0.001秒是1毫秒，請自行換算。

使用時會有Exception，要強制處理Exception。
{% highlight java linenos %}
  try {
    Thread.sleep(1000);  // 休息1秒
  } catch (InterruptedException e) {
    // 要處理異常
  }
{% endhighlight %}

## 阻斷與同步
阻斷的英文是Blocked

同步的英文是synchronous

下面的程式碼，在main()方法中呼叫func1()，func1()執行完，再回到main()方法，執行呼叫func1()的下一行。

main()在呼叫func1()的時候，是被卡住(Block)，必須等到func1()執行完，才能繼續執行，這個卡住的過程，稱為阻斷。

同步(synchronous)就是等待上一個動作完成，才可以處理下一個動作，在這裡的例子是main()必須等待func1()執行完，才能執行呼叫func1()的下一行。

{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    // 呼叫func1()
    func1();

    System.out.println("回到main方法");
    System.out.println("執行func1()之後的程式碼");
  }

  public static void func1 () {
    System.out.println("執行func1()");
  }
}
{% endhighlight %}
```
執行func1()
回到main方法
執行func1()之後的程式碼
```
## 執行緒與主程式同時執行
執行緒與main執行緒是同時執行，main不等待。

下圖中，程序呼叫主要執行緒(Main Thread)，main啟動執行緒1，但main執行結束，執行緒1仍在執行，直到程序中所有執行緒都結束了，程序才結束。

![img]({{site.imgurl}}/java/thread2.png)

## 建立Thread
建立Thread有二個方式，一個是繼承thread類別，一個是實作Runnable介面。

### 繼承thread類別
Java不支援多繼承，繼承Thread後就沒辦法繼承其它類別，建議使用Runnable，比較不會有限制。

類別繼承Thread，並覆寫run()方法。
{% highlight java linenos %}
public class 類別名 extends Thread {
  @Override
  public void run() {
    // 要執行的流程
  }
}
{% endhighlight %}

啟動執行緒
{% highlight java linenos %}
new 類別名().start();
{% endhighlight %}

程式碼
{% highlight java linenos %}
class Thread1 extends Thread {
  @Override
  public void run() {
    // 每1秒印1句話，執行10秒
    for (int i = 0; i < 10; i++) {
      System.out.println("Thread1 i = " + i);
      try {
        // 休息1秒
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }

    System.out.println("Thread1 執行緒結束");
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws InterruptedException {
    // 建立執行緒
    Thread1 t1 = new Thread1();
    // 啟動執行緒
    t1.start();

    // 每1秒印1句話，執行5秒
    for (int i = 0; i < 5; i++) {
      System.out.println("main i = " + i);
      // 休息1秒
      Thread.sleep(1000);
    }

    System.out.println("main 主要執行緒結束");
  }
}
{% endhighlight %}
```
main i = 0
Thread1 i = 0
Thread1 i = 1
main i = 1
Thread1 i = 2
main i = 2
Thread1 i = 3
main i = 3
Thread1 i = 4
main i = 4
main 主要執行緒結束
Thread1 i = 5
Thread1 i = 6
Thread1 i = 7
Thread1 i = 8
Thread1 i = 9
Thread1 執行緒結束

Process finished with exit code 0
```
由執行結果可以發現main與Thread1同時執行，而且main比Thread1提早結束。

Thread1執行結束，程序才結束。

### 實作Runnable介面
類別實作Runnable介面，並覆寫run()方法。
{% highlight java linenos %}
public class 類別名 implements Runnable {
  @Override
  public void run() {
    // 要執行的流程
  }  
}
{% endhighlight %}

先前Thread原始碼有提到，Thread類別才有start()方法，要透過start()方法啟動執行緒。

所以要把實作Runnable物件放入Thread有參數的建構子。
{% highlight java linenos %}
public class Thread implements Runnable {
    // 參數是有實作Runnable的類別
    public Thread(Runnable task) {
        this(null, null, 0, task, 0, null);
    }
}
{% endhighlight %}

把Runable物件放進Thread的有參數的建構子，然後使用Thread類別start()方法啟動執行緒。
{% highlight java linenos %}
new Thread(new Runnable物件()).start();
{% endhighlight %}

程式碼，把上一個程式碼修改成實作Runnable。
{% highlight java linenos %}
class R1 implements Runnable {
  @Override
  public void run() {
    for (int i = 0; i < 10; i++) {
      System.out.println("R1 i = " + i);
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }
    System.out.println("R1 執行緒結束");
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws InterruptedException {
    // 建立Runnable物件
    R1 r1 = new R1();
    // 把Runnable物件放入Thread的建構子
    Thread t1 = new Thread(r1);
    // 啟動執行緒
    t1.start();
    for (int i = 0; i < 5; i++) {
      System.out.println("main i = " + i);
      Thread.sleep(1000);
    }
    System.out.println("main 主要執行緒結束");
  }
}
{% endhighlight %}

## 匿名類別
### Runnable匿名類別
要把實作Runnable匿名物件放入Thread有參數的建構子。
{% highlight java linenos %}
new Thread(new Runnable() {
  @Override
  public void run() { ... }
}).start();
{% endhighlight %}

### thread匿名類別
使用無參數的建構子。
{% highlight java linenos %}
new Thread() {
  @Override
  public void run() {
    // 要執行的流程
  }
}.start();
{% endhighlight %}

## 結束執行緒
### return
使用return，就會結束執行緒。
{% highlight java linenos %}
class Thread1 implements Runnable {
  private int count = 0;
  @Override
  public void run() {
    for (int i = 0; i < 10; i++) {
      for (int j = 0; j < 3; j++) {
        System.out.println("Thread1 count  = " + (++count));
        try {
          Thread.sleep(1000);
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
        // 若count等於5，就結束執行緒
        if( count == 5) return;
      }
    }
    System.out.println("Thread1 執行緒結束");
  }
}
{% endhighlight %}
```
Thread1 count  = 1
main i = 0
main i = 1
Thread1 count  = 2
main i = 2
Thread1 count  = 3
main i = 3
Thread1 count  = 4
main i = 4
Thread1 count  = 5
main 主要執行緒結束
```
由執行結果發現，執行到count = 5，Thread1就結束了。

### break
離開迴圈也是結束執行緒的方法，如果只有1層迴圈，可以離開執行緒，2層迴圈使用break，是只跳出目前的迴圈，並非離開函式。
{% highlight java linenos %}
class Thread1 implements Runnable {
  private int count = 0;
  @Override
  public void run() {
    for (int i = 0; i < 10; i++) {
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      // 若count等於5，就結束執行緒
      if( count == 5) break;
    }
    System.out.println("Thread1 執行緒結束");
  }
}
{% endhighlight %}

### 使用flag
提供set方法，設定私有屬性flag。

由其它類別控制何時結束。
{% highlight java linenos %}
class R2 implements Runnable {
  // 控制結束的flag
  private boolean flag = true;

  // 由其它類別來設定flag
  public void setFlag(boolean flag) {
    this.flag = flag;
  }

  @Override
  public void run() {
    while (flag) {
      System.out.println(Thread.currentThread().getName() + "執行中");
      // 每秒印1次
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }
  }
}
{% endhighlight %}


{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) throws InterruptedException {
    R2 r2 = new R2();
    // 啟動執行緒
    new Thread(r2).start();

    for (int i = 0; i < 10; i++) {
      // i為5就把執行緒關掉
      if(i == 5) {
        r2.setFlag(false);
      }
      // 每秒印1次
      System.out.println("i = " + i);
      Thread.sleep(1000);
    }
  }
}
{% endhighlight %}
```
Thread-0執行中
i = 0
Thread-0執行中
i = 1
Thread-0執行中
i = 2
Thread-0執行中
i = 3
Thread-0執行中
i = 4
Thread-0執行中
i = 5
i = 6
i = 7
i = 8
i = 9
```
## Thread類別常用方法
### setName()
設定緒行緒的名字。

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws InterruptedException {
    // 建立執行緒物件
    Thread1 t1 = new Thread1();
    //設定執行緒的名字
    t1.setName("執行緒1");
    // 啟動執行緒
    t1.start();
  }
}
{% endhighlight %}

### currentThread().getName()
取得目前執行緒的名字。

{% highlight java linenos %}
class R1 implements Runnable {
  @Override
  public void run() {
    for (int i = 0; i < 10; i++) {
      // 取得目前執行緒的名字。
      System.out.println(Thread.currentThread().getName() + " i  = " + i);
    }
  }
}
{% endhighlight %}

在main中取得執行緒的名字。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(Thread.currentThread().getName());
  }
}
{% endhighlight %}
```
main
```

## 執行緒共享
所謂的共享是不同的執行緒(Thread)，存取相同的記憶體位址。

### Runnable共享
只要建立一個Runnable物件，多個執行緒可以共享同一個Runnable物件的變數。

同一個物件建立的執行緒，多個執行緒可使用相同的成員屬性。

![img]({{site.imgurl}}/java/thread_share.png)

R1是實作Runnable的物件。
{% highlight java linenos %}
class R1 implements Runnable {
  // 執行緒使用相同屬性money
  private int money = 10000;
  @Override
  public void run() {
    // 無窮迴圈
    while (true) {
      if (money <= 0) break;
      money = money - 1000;
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    }
  }
}
{% endhighlight %}

同時啟動2個執行緒。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 建立Runnable物件
    R1 r1 = new R1();

    // 使用r1 Runnable物件為參數
    Thread t1 = new Thread(r1);
    // 設定名字
    t1.setName("執行緒1");

    // 使用r1 Runnable物件為參數
    Thread t2 = new Thread(r1);
    // 設定名字
    t2.setName("執行緒2");

    // 啟動執行緒
    t1.start();
    t2.start();
  }
}
{% endhighlight %}
```
執行緒2 提領 1000 元，剩下 8000 元
執行緒2 提領 1000 元，剩下 7000 元
執行緒2 提領 1000 元，剩下 6000 元
執行緒2 提領 1000 元，剩下 5000 元
執行緒2 提領 1000 元，剩下 4000 元
執行緒1 提領 1000 元，剩下 9000 元
執行緒1 提領 1000 元，剩下 2000 元
執行緒1 提領 1000 元，剩下 1000 元
執行緒1 提領 1000 元，剩下 0 元
執行緒2 提領 1000 元，剩下 3000 元
```

因為沒有使用synchronized，所以執行結果不如預期，但二個執行緒都使用相同money屬性。

### 繼承Thread共享資源
繼承Thread沒辦法共用相同物件屬性，因為每一次都是用new Thread()，每一次new都是不同物件。
{% highlight java linenos %}
  // 建立執行緒
  Thread1 t1 = new Thread1();
  // 啟動執行緒
  t1.start();

  // 建立執行緒
  Thread1 t2 = new Thread1();
  // 啟動執行緒
  t2.start();
  
  // 建立執行緒
  Thread1 t3 = new Thread1();
  // 啟動執行緒
  t3.start();        
{% endhighlight %}

只能使用以下二種方法，存取相同的記憶體位址。
1. Static靜態變數
2. 提供一個public方法或建構子，傳入相同物件，才能存取相同的記憶體位址。

### 使用Static變數
{% highlight java linenos %}
class TestThread extends Thread {
  private static int money = 10000;
  @Override
  public void run() {
    while (true) {
      if (money <= 0) break;
      money = money - 1000;
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    }
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    TestThread t1 = new TestThread();
    t1.setName("執行緒1");

    TestThread t2 = new TestThread();
    t2.setName("執行緒2");

    //啟動執行緒
    t1.start();
    t2.start();
  }
}
{% endhighlight %}
```
執行緒2 提領 1000 元，剩下 8000 元
執行緒2 提領 1000 元，剩下 7000 元
執行緒2 提領 1000 元，剩下 6000 元
執行緒2 提領 1000 元，剩下 5000 元
執行緒2 提領 1000 元，剩下 4000 元
執行緒2 提領 1000 元，剩下 3000 元
執行緒2 提領 1000 元，剩下 2000 元
執行緒2 提領 1000 元，剩下 1000 元
執行緒2 提領 1000 元，剩下 0 元
執行緒1 提領 1000 元，剩下 9000 元
```

### 使用物件
建立提款卡類別，裡面有10000元。
{% highlight java linenos %}
class BankCard {
  private int money = 10000;

  public int getMoney() {
    return money;
  }

  public void setMoney(int money) {
    this.money = money;
  }
}
{% endhighlight %}

執行緒提供參數為提款卡的建構子。
{% highlight java linenos %}
class TestThread extends Thread {
  private BankCard card;

  public TestThread(BankCard card) {
    this.card = card;
  }

  @Override
  public void run() {
    while (true) {
      if (card.getMoney() <= 0) break;
      card.setMoney(card.getMoney() - 1000);
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + card.getMoney() + " 元");
    }
  }
}
{% endhighlight %}

啟動執行緒。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    BankCard bankCard = new BankCard();
    // 放入相同的bankCard物件
    TestThread t1 = new TestThread(bankCard);
    t1.setName("執行緒1");

    // 放入相同的bankCard物件
    TestThread t2 = new TestThread(bankCard);
    t2.setName("執行緒2");

    // 啟動
    t1.start();
    t2.start();
  }
}
{% endhighlight %}
```
執行緒2 提領 1000 元，剩下 8000 元
執行緒2 提領 1000 元，剩下 7000 元
執行緒1 提領 1000 元，剩下 9000 元
執行緒1 提領 1000 元，剩下 5000 元
執行緒2 提領 1000 元，剩下 6000 元
執行緒1 提領 1000 元，剩下 4000 元
執行緒2 提領 1000 元，剩下 3000 元
執行緒1 提領 1000 元，剩下 2000 元
執行緒2 提領 1000 元，剩下 1000 元
執行緒1 提領 1000 元，剩下 0 元
```

因為沒有使用synchronized，所以執行結果不如預期，但二個執行緒都使用相同bankCard物件。

## 呼叫run()
先前已經提過，呼叫run()，等同於呼叫方法，不是啟動執行緒。

呼叫方法，會有阻斷Block，也就是前面的方法執行完，才能執行接下來的方法，按照順序執行。

{% highlight java linenos %}
class TestThread2 extends Thread {
  @Override
  public void run() {
    for (int i = 0; i < 5; i++) {
      System.out.println(Thread.currentThread().getName() + " i = " + i);
    }
  }
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    TestThread2 t1 = new TestThread2();
    t1.setName("執行緒1");

    TestThread2 t2 = new TestThread2();
    t2.setName("執行緒2");

    t1.run();
    t2.run();
  }
}
{% endhighlight %}
```
main i = 0
main i = 1
main i = 2
main i = 3
main i = 4
main i = 0
main i = 1
main i = 2
main i = 3
main i = 4
```
由執行結果可以發現，雖然有設定執行緒的名字，但印出來的執行緒名字都是main，代表run()都是在main執行緒中執行。

而且印出的i是0至4，共印出2次，也就是呼叫2次run()。

[1]: {% link _pages/java/anonymous_class.md %}
[2]: {% link _pages/java/interface.md %}