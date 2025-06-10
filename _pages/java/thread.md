---
title: Thread
date: 2025-04-07
keywords: Android, java, thread, Runnable
---
Prerequisites:

- [匿名內部類別][1]
- [介面][2]

## 名詞介紹
程式碼(Program)透過javac產生了.class檔，.class檔又稱為位元碼。
```
javac 類別.java
```

程序(Process)把.class位元碼載入到記憶體，從開始執行到執行完畢的一個過程。
```
java 類別
```

可以下很多次`java 類別`指令，執行多個程序。

![img]({{site.imgurl}}/java/process1.png)

操作系統(OS)可以管理多個程序(Process)，可以讓多個程序(Process)輪流使用CPU資源。

Jvm視為虛擬的操作系統，管理多個程序中的多個執行緒。

每一個執行緒(Thread)都是一個流程。

每個程序(Process)都會有一個主要執行緒(Main Thread)也稱作主要流程，是一個程序執行的入口。

為了執行效率提升，所以想「同時執行」很多流程，每一個流程都是一個執行緒，main執行緒建立了2個執行緒，其中一個執行緒，又再建立2個執行緒，建立了很多執行緒(Thread)，大家來「同時」執行。

![img]({{site.imgurl}}/java/thread1.png)

### Concurrent併發
一個CPU執行多個執行緒，Jvm有一個排程管理系統，會十分快速的將一個流程(執行緒)切換到另一個流程(執行緒)，快速的切換讓使用者以為是同時執行這些流程(執行緒)，實際上是十分快速的輪流執行(執行緒)。

![img]({{site.imgurl}}/java/cpu1.png)

### Parallel
多個CPU執行多個執行緒，平行執行任務。

![img]({{site.imgurl}}/java/cpu2.png)

## 建立Thread
建立Thread有二個方式，一個是繼承thread類別，一個是實作Runnable介面。

### Runnable介面
類別實作Runnable介面，並覆寫run()方法。
{% highlight java linenos %}
public class 類別名 implements Runnable {
  @Override
  public void run() {
    // 要執行的流程
  }  
}
new Thread(new 類別名());
{% endhighlight %}

執行
{% highlight java linenos %}
new Thread(new 類別名()).start();
{% endhighlight %}

### 繼承thread類別
類別繼承Thread，並覆寫run()方法。
{% highlight java linenos %}
public class 類別名 extends Thread {
  @Override
  public void run() {
    // 要執行的流程
  }
}
{% endhighlight %}

執行
{% highlight java linenos %}
new 類別名().start();
{% endhighlight %}

## 匿名類別
### Runnable匿名類別
實作Runnable匿名類別
{% highlight java linenos %}
new Thread(new Runnable() {
  @Override
  public void run() {
    // 要執行的流程
  }
})
{% endhighlight %}

執行語法
{% highlight java linenos %}
new Thread(new Runnable() {
  @Override
  public void run() { ... }
}).start()
{% endhighlight %}

### thread匿名類別
繼承thread匿名類別
{% highlight java linenos %}
new Thread() {
  @Override
  public void run() {
    // 要執行的流程
  }
};
{% endhighlight %}

執行
{% highlight java linenos %}
new Thread() {
  @Override
  public void run() {
    // 要執行的流程
  }
}.start();
{% endhighlight %}

## 執行緒共享資源
所謂的共享是不同的執行緒(Thread)使用相同的變數(相同的記憶體位址)，不同的執行緒對相同的變數進行存取動作。

### Runnable共享資源
只要建立一個Runnable物件，多個執行緒可以共享同一個Runnable物件的變數，類別可以實作很多介面interface。

Runnable物件:AntR.java
{% highlight java linenos %}
public class AntR implements Runnable{
  int cake;
  public void setCake(int c) {
    cake = c;
  }
  @Override
  public void run() {
    int n = 2;
    while (true) {
      if (cake <= 0) {
        System.out.println(Thread.currentThread().getName() + " 死亡");
        //!!!十分重要，結束循環
        return;
      }
      // 多個執行緒使用同一個cake變數
      cake = cake - n;
      System.out.println(Thread.currentThread().getName() + "吃" + n + "g，蛋糕剩下" + cake + "g");
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
public static void main(String[] args) {
  // 只要建立一個Runnable物件
  AntR runnableObj = new AntR();
  runnableObj.setCake(10);
  // 參數用同一個Runnable物件
  Thread antRed = new Thread(runnableObj);
  antRed.setName("紅螞蟻");
  // 參數用同一個Runnable物件
  Thread antBlack = new Thread(runnableObj);
  antBlack.setName("黑螞蟻");
  antRed.start();
  antBlack.start();
}
{% endhighlight %}
```
紅螞蟻吃2g，蛋糕剩下8g
黑螞蟻吃2g，蛋糕剩下6g
黑螞蟻吃2g，蛋糕剩下2g
紅螞蟻吃2g，蛋糕剩下4g
黑螞蟻吃2g，蛋糕剩下0g
紅螞蟻 死亡
黑螞蟻 死亡
```

### 繼承Thread共享資源
繼承Thread的優點是可以共享成員變數與成員方法，缺點是Java不支援多繼承，繼承Thread後就沒辦法繼承其它類別。

Cake.java
{% highlight java linenos %}
public class Cake {
  int size;
  public void setSize(int size) {
    this.size = size;
  }
  public int getSize() {
    return size;
  }
  public void lost(int n) {
    if (size - n >= 0) {
      size = size - n;
    }
  }
}
{% endhighlight %}

Ant.java
{% highlight java linenos %}
public class Ant extends Thread{
  Cake cake;
  public Ant(String name, Cake cake) {
    this.cake = cake;
    setName(name);
  }
  @Override
  public void run() {
    while (true) {
      int n = 2;
      cake.lost(n);
      System.out.println(getName()+" 吃 " + n + "g蛋糕, 蛋糕剩下"+ cake.getSize() +"g");
      try {
        sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      if (cake.getSize() <= 0) {
        System.out.println(getName() + "已死亡");
        //!!!十分重要，結束循環
        return;
      }
    }
  }
}
{% endhighlight %}

{% highlight java linenos %}
public static void main(String[] args) {
  Cake cake = new Cake();
  cake.setSize(10);
  Ant antRed = new Ant("red ant", cake);
  Ant antBlack = new Ant("Black ant", cake);
  antRed.start();
  antBlack.start();
}
{% endhighlight %}
```
Black ant 吃 2g蛋糕, 蛋糕剩下6g
red ant 吃 2g蛋糕, 蛋糕剩下8g
Black ant 吃 2g蛋糕, 蛋糕剩下4g
red ant 吃 2g蛋糕, 蛋糕剩下2g
Black ant 吃 2g蛋糕, 蛋糕剩下0g
red ant已死亡
Black ant已死亡
```

## 同步與非同步
### 同步synchronous
等待上一個動作完成，才可以處理下一個動作。

### 非同步asynchronous
不用等待上一個動作完成，可以同時做多個動作。  
多個流程或稱執行緒(thread)同時進行，不用特別等上一個流程(執行緒)完成，才能進行下一個流程(執行緒)。

## 同步鎖lock

非同步(asynchronous)的流程中，共享資源時造成變數值不一致，因為多個流程(執行緒thread)同時存取或修改同一個變數，所以需要使用同步鎖(lock)，必須把目前的流程(thread)處理完，才輪下一個流程(thread)處理，還原成同步的狀況，所有流程(thread)排隊，等待進入廁所(要同步的區域)，只有拿到鎖(lock)的流程(thread)，才能進入廁所，鎖上(lock)廁所門，上完廁所(執行完)，把廁所門的鎖打開(unlock)，輪到下一個排隊的執行緒(thread)去上廁所。

![img]({{site.imgurl}}/toilet.png)  


[1]: {% link _pages/java/anonymous_class.md %}
[2]: {% link _pages/java/interface.md %}