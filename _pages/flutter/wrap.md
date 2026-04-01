---
title: Wrap
date: 2026-04-01
keywords: flutter, widget, Wrap
---
Wrap有自動換行的特性。<br>

|屬性|說明|
|:----:|:---------------------|
|direction 排列方向|Axis.vertical垂直，Axis.horizontal水平|
|spacing|主軸間距|
|runSpacing|交叉軸間距|
|alignment|主軸對齊方式|
|runAlignment|交叉軸對齊方式|
|children|子元素|

## direction
### Axis.vertical 垂直排列
- spacing 上下間距
- runSpacing 左右間距
- alignment 垂直spaceBetween、spaceAround、spaceEvenly、(上)start、(下)end
- runAlignment 靠左start、靠右end、置中center

### Axis.horizontal 水平排列
- spacing 左右間距
- runSpacing 上下間距
- alignment 靠左start、靠右end、置中center
- runAlignment 垂直spaceBetween、spaceAround、spaceEvenly、(上)start、(下)end

以下程式碼
- direction主軸: Axis.horizontal 水平
- alignment: WrapAlignment.end 水平靠右對齊
- runAlignment: WrapAlignment.spaceBetween 垂直對齊上下各一個，中間元素置中。

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  List<Widget> getList() {
    return List.generate(
        10, // 數量
        (index) => Container(
              color: Colors.grey,
              width: 100,
              height: 100,
            ));
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: Container(
        color: Colors.amber,
        width: double.infinity,
        height: double.infinity,
        child: Wrap(
          direction: Axis.horizontal,
          spacing: 20,
          runSpacing: 20,
          alignment: WrapAlignment.end,
          runAlignment: WrapAlignment.spaceBetween,
          children: getList(),
        ),
      ),
    ));
  }
}

{% endhighlight %}

![img]({{site.imgurl}}/flutter/wrap1.png)<br>