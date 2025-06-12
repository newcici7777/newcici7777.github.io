---
title: synchronized
date: 2025-06-12
keywords: Android, java, synchronized, thread
---
## 同步流程與非同步流程
### 同步流程synchronous
等待上一個動作完成，才可以處理下一個動作。

### 非同步流程asynchronous
不用等待上一個動作完成，可以同時做多個動作。  
多個流程同時執行或稱執行緒(thread)同時執行，不用特別等上一個流程(執行緒)完成，才能進行下一個流程(執行緒)。

## synchronized同步鎖
非同步(asynchronous)的流程中，共享資源時造成變數值不一致，因為多個流程(執行緒thread)同時存取或修改同一個變數，所以需要使用同步鎖(synchronized)，必須把目前的流程(thread)處理完，才輪下一個流程(thread)處理，還原成同步的狀況。

想像一下，只有一個廁所，所有執行緒都在門外排隊，只有一個執行緒能上廁所，上廁所前把門鎖上，上完廁所(執行完)，把廁所門的鎖打開(unlock解鎖)，輪到下一個執行緒去上廁所。

而這個鎖上門的動作，稱為同步鎖(synchronized)。

而只有一個執行緒上廁所，這個一個人獨享上廁所過程，稱為同步(synchronized)方法或同步(synchronized)區塊。

也就是說，如果某個流程只限定只讓一個執行緒使用，就要把這段流程使用同步(synchronized)方法或同步區塊包住。

## 阻斷Block
只有一個執行緒能上廁所，其它執行緒在外面等待，上完廁所，再輪下一個執行緒上廁所，這個過程叫作阻斷Block。

![img]({{site.imgurl}}/toilet.png)  

## synchronized 方法
在方法前加上synchronized，代表一次只能一個執行緒執行這個方法，但執行速度會變慢，因為一次只能一個執行緒使用方法，其它執行緒都在方法外等待使用。

使用synchronized，有下面幾點技巧，請follow以下步驟。

1.把要同步的流程，移到synchronized 「新方法」中，不要在run()方法前面加上synchronized，這樣做的話，多個執行緒啟動，只有第1個執行緒啟動。

{% highlight java linenos %}
  public synchronized void run() {
  }
{% endhighlight %}

2.使用flag來離開執行緒。

3.迴圈或Looper留在run()方法中。

{% highlight java linenos %}
  @Override
  public void run() {
    while (flag) {
    }
  }
{% endhighlight %}

4.在synchronized 「新方法」中，要用return，離開方法。

以上步驟完成的結果如下:
{% highlight java linenos %}
class R1 implements Runnable {
  private int money = 10000;
  // 2.使用flag來離開執行緒
  private boolean flag = true;

  // 1.把要同步的流程，移到synchronized 「新方法」中
  private synchronized void method1() {
    if (money <= 0) {
      // 2.使用flag來離開執行緒
      flag = false;
      // 4.在synchronized 「新方法」中，要用return，離開方法
      return;
    }
    money = money - 1000;

    try {
      // 每秒領一次錢
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
  }

  @Override
  public void run() {
    // 迴圈或Looper留在run()方法中。
    while (flag) {
      // 呼叫synchronized 「新方法」
      method1();
    }
  }
}
{% endhighlight %}

測試程式。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 建立Runnable物件
    R1 r1 = new R1();
    // 建立執行緒
    Thread t1 = new Thread(r1);
    t1.setName("執行緒1");
    Thread t2 = new Thread(r1);
    t2.setName("執行緒2");
    // 啟動執行緒
    t1.start();
    t2.start();
  }
}
{% endhighlight %}
```
執行緒1 提領 1000 元，剩下 9000 元
執行緒2 提領 1000 元，剩下 8000 元
執行緒2 提領 1000 元，剩下 7000 元
執行緒2 提領 1000 元，剩下 6000 元
執行緒2 提領 1000 元，剩下 5000 元
執行緒2 提領 1000 元，剩下 4000 元
執行緒2 提領 1000 元，剩下 3000 元
執行緒2 提領 1000 元，剩下 2000 元
執行緒2 提領 1000 元，剩下 1000 元
執行緒2 提領 1000 元，剩下 0 元
```

## synchronized(this)同步區塊
synchronized(this)是鎖住目前物件屬性，使用在非靜態方法。

`synchronized(this){}`，被花括號`{}`包住的部分叫同步區塊，比synchronized方法更好的地方在於，只有包住的區塊，只提供一次給一個執行緒執行，未在同步區塊中的程式碼，可被多個執行緒使用。

語法
```
synchronized(this) {
}
```

改成使用synchronized(this)區塊。
{% highlight java linenos %}
class R1 implements Runnable {
  private int money = 10000;
  private boolean flag = true;

  private void method1() {
    // 使用this同步區塊
    synchronized(this) {
      if (money <= 0) {
        flag = false;
        // 離開方法
        return;
      }
      money = money - 1000;
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    }
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    while (flag) {
      method1();
    }
  }
}
{% endhighlight %}

## synchronized(object)區塊
除了this之外，也可以使用其它物件，只要確認多個執行緒中，使用的object是同一個。
{% highlight java linenos %}
class R1 implements Runnable {
  private int money = 10000;
  private boolean flag = true;
  // 建立Object物件
  private Object obj;
  
  // 由其它類別設定Object
  public void setObj(Object obj) {
    this.obj = obj;
  }
  
  private void method1() {
    // 使用Object同步區塊
    synchronized(obj) {
      if (money <= 0) {
        flag = false;
        // 離開方法
        return;
      }
      money = money - 1000;
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    }
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    while (flag) {
      method1();
    }
  }
}
{% endhighlight %}

測試執行緒。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 建立Object
    Object object = new Object();
    // 建立Runnable物件
    R1 r1 = new R1();
    r1.setObj(object);

    Thread t1 = new Thread(r1);
    t1.setName("執行緒1");
    Thread t2 = new Thread(r1);
    t2.setName("執行緒2");

    t1.start();
    t2.start();
  }
}
{% endhighlight %}

也可以像下面這樣寫，因為擁有相同Runnable物件的多個執行緒，都是使用同一個Runnable物件，所以以下的寫法，也是所有執行共同使用obj物件。
{% highlight java linenos %}
class R1 implements Runnable {
  private int money = 10000;
  private boolean flag = true;
  // 改成new R1的時候，直接new Object屬性。
  private Object obj = new Object();
  
  private void method1() {
    synchronized (obj) {
      if (money <= 0) {
        flag = false;
        // 離開方法
        return;
      }
      money = money - 1000;
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    }
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    while (flag) {
      method1();
    }
  }
}

{% endhighlight %}

## synchronized(BankCard物件)區塊
把之前在Thread文章中，使用的BankCard範例按照之前的步驟修改，並使用BankCard物件作為同步區塊。
{% highlight java linenos %}
class TestThread extends Thread {
  private BankCard card;
  // 2. 使用flag來離開執行緒。
  private boolean flag = true;

  public TestThread(BankCard card) {
    this.card = card;
  }

  // 1. 把要同步的流程，移到「新方法」中
  public void method1 () {
    // synchronized (BankCard物件)
    synchronized (card) {
      if (card.getMoney() <= 0) {
        // 2. 使用flag來離開執行緒。
        flag = false;
        // 4. 在「新方法」中，要用return，離開方法。
        return;
      }
      card.setMoney(card.getMoney() - 1000);
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + card.getMoney() + " 元");
    }
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    // 3. 迴圈或Looper留在run()方法中。
    while (flag) {  // 2. 使用flag來離開執行緒。
      method1();
    }
  }
}
{% endhighlight %}

每個執行緒都使用相同bankCard物件。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 建立bankCard物件
    BankCard bankCard = new BankCard();

    // 確保每個執行緒都使用同一個bankCard物件
    TestThread t1 = new TestThread(bankCard);
    t1.setName("執行緒1");

    // 確保每個執行緒都使用同一個bankCard物件
    TestThread t2 = new TestThread(bankCard);
    t2.setName("執行緒2");

    t1.start();
    t2.start();
  }
}
{% endhighlight %}
```
執行緒1 提領 1000 元，剩下 9000 元
執行緒2 提領 1000 元，剩下 8000 元
執行緒2 提領 1000 元，剩下 7000 元
執行緒1 提領 1000 元，剩下 6000 元
執行緒2 提領 1000 元，剩下 5000 元
執行緒1 提領 1000 元，剩下 4000 元
執行緒2 提領 1000 元，剩下 3000 元
執行緒1 提領 1000 元，剩下 2000 元
執行緒2 提領 1000 元，剩下 1000 元
執行緒1 提領 1000 元，剩下 0 元
```

以下的程式碼，就不是同步鎖，因為bankCard都不一樣，同步鎖會失效。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 每個執行緒的BankCard物件都是不一樣。
    TestThread t1 = new TestThread(new BankCard());
    t1.setName("執行緒1");

    TestThread t2 = new TestThread(new BankCard());
    t2.setName("執行緒2");

    t1.start();
    t2.start();
  }
}
{% endhighlight %}

## synchronized(類別.class)靜態同步區塊
針對static變數，要使用類別同步鎖，而不是this物件，this鎖的是物件，對類別的變數與類別的方法是沒有用的。

靜態變數與靜態方法，是屬於類別，不是物件，所以要鎖類別。
{% highlight java linenos %}
class TestThread2 extends Thread {
  private boolean flag = true;
  // 靜態變數
  private static int money = 10000;
  public void method1() {
    // 鎖的是類別
    synchronized (TestThread2.class) {
      if (money <= 0) {
        flag = false;
        return;
      }
      money = money - 1000;
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    }
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }
  @Override
  public void run() {
    while (flag) {
      method1();
    }
  }
}
{% endhighlight %}

測試執行緒
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    TestThread2 t1 = new TestThread2();
    t1.setName("執行緒1");
    TestThread2 t2 = new TestThread2();
    t2.setName("執行緒2");
    TestThread2 t3 = new TestThread2();
    t3.setName("執行緒3");
    t1.start();
    t2.start();
    t3.start();
  }
}
{% endhighlight %}
```
執行緒1 提領 1000 元，剩下 9000 元
執行緒3 提領 1000 元，剩下 8000 元
執行緒2 提領 1000 元，剩下 7000 元
執行緒1 提領 1000 元，剩下 6000 元
執行緒2 提領 1000 元，剩下 5000 元
執行緒3 提領 1000 元，剩下 4000 元
執行緒1 提領 1000 元，剩下 3000 元
執行緒3 提領 1000 元，剩下 2000 元
執行緒2 提領 1000 元，剩下 1000 元
執行緒1 提領 1000 元，剩下 0 元
```

## 靜態同步區塊
在靜態方法中，使用靜態同步區塊。
{% highlight java linenos %}
class TestThread2 extends Thread {
  // 全變成靜態
  private static boolean flag = true;
  private static int money = 10000;

  // 靜態方法
  public static void method1() {
    synchronized (TestThread2.class) {
      if (money <= 0) {
        flag = false;
        return;
      }
      money = money - 1000;
      System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    }
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    while (flag) {
      method1();
    }
  }
}
{% endhighlight %}

## 靜態同步方法
{% highlight java linenos %}
class TestThread2 extends Thread {
  private static boolean flag = true;
  private static int money = 10000;

  // synchronized在靜態方法
  public synchronized static void method1() {
    if (money <= 0) {
      flag = false;
      return;
    }
    money = money - 1000;
    System.out.println(Thread.currentThread().getName() + " 提領 1000 元，剩下 " + money + " 元");
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    while (flag) {
      method1();
    }
  }
}
{% endhighlight %}