---
title: Color 與 decoration
date: 2026-04-04
keywords: flutter, widget, Color, decoration
---
decoration 與 color 二個屬性不能同時存在，二者只能選一個。<br>
{% highlight dart linenos %}
Container(
    decoration: BoxDecoration(
      color: Colors.red
    ),
    width: 100,
    height: 100,
    color: Colors.red,
    child: Text("底部"),
)
{% endhighlight %}

![img]({{site.imgurl}}/flutter/container_error.png)<br>

Color只有背景顏色的功能。<br>

但decoration可以有背景顏色、邊框、邊框顏色、圓角、漸層..等等特效。<br>

## BoxDecoration
{% highlight dart linenos %}
Container(
    decoration: BoxDecoration(
      // 顏色
      color: Colors.red,
      // 圓角
      borderRadius: BorderRadius.circular(40),
    ),
    width: 100,
    height: 100,
    alignment: Alignment.center,
    child: Text("底部"),
)
{% endhighlight %}

## InputDecoration
![img]({{site.imgurl}}/flutter/textfield1.png)<br>

{% highlight dart linenos %}
TextField(
  decoration: InputDecoration(
      // 內部間距
      contentPadding: EdgeInsets.only(left: 20.0, right: 10),
      hintText: "請輸入姓名",
      // 輸入方塊背景顏色
      fillColor: Colors.grey,
      filled: true,
      border: OutlineInputBorder(
      	  // 無邊框
          borderSide: BorderSide.none,
          // 圓角
          borderRadius: BorderRadius.circular(10.0))))
{% endhighlight %}