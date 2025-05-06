---
title: 樣板模式
date: 2025-05-06
keywords: Java, Design patterns, Template pattern
---
樣板模式的重點，把不變的內容放在抽象父類別，會變的放在子類別。

## 什麼是樣板模式？
想像成問卷，題目是一樣，把一樣的題目放在父類別。

但每個人回答的答案是不一樣的，不一樣的地方，就由子類別自己去覆寫。

## 父類別放一樣的內容
{% highlight java linenos %}
public abstract class Question {
  public void getQuestion() {
    // 題目都一樣
    System.out.println("你最喜歡的夜市美食是什麼？");
    // 回答內容不一樣，由子類別覆寫
    answer();
  }

  // 回答內容不一樣，由子類別覆寫
  public abstract void answer();
}
{% endhighlight %}

## 子類別覆寫不一樣的內容
Answer1
{% highlight java linenos %}
public class Answer1 extends Question{
  @Override
  public void answer() {
    System.out.println("蚵仔煎");
  }
}
{% endhighlight %}

Answer2
{% highlight java linenos %}
public class Answer2 extends Question{
  @Override
  public void answer() {
    System.out.println("臭豆腐");
  }
}
{% endhighlight %}

Answer3
{% highlight java linenos %}
public class Answer3 extends Question{
  @Override
  public void answer() {
    System.out.println("雞排");
  }
}
{% endhighlight %}

## Client測試
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) {
    Question ans1 = new Answer1();
    ans1.getQuestion();
    System.out.println("=======================");

    Question ans2 = new Answer2();
    ans2.getQuestion();
    System.out.println("=======================");

    Question ans3 = new Answer3();
    ans3.getQuestion();
    System.out.println("=======================");
  }
}
{% endhighlight %}
```
你最喜歡的夜市美食是什麼？
蚵仔煎
=======================
你最喜歡的夜市美食是什麼？
臭豆腐
=======================
你最喜歡的夜市美食是什麼？
雞排
=======================
```