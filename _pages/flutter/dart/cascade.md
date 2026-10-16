---
title: ..Cascade
date: 2026-10-05
keywords: flutter, Cascade
---
{% highlight dart linenos %}
// 傳統寫法
var robot = Robot();
robot.name = "小明";
robot.level = 10;
robot.wakeUp();

// 用 .. 串接寫法 (簡潔很多)
var robot = Robot()
  ..name = "小明"
  ..level = 10
  ..wakeUp();
{% endhighlight %}


{% highlight dart linenos %}
var listenController = TextEditingController()
  ..text = updatedOptions.listenFor.toString();
{% endhighlight %}

意思就是：「建立一個 TextEditingController，順便把它的 text 屬性設為預設數值的字串，然後把這個設定好的 controller 指定給 listenController 變數。」