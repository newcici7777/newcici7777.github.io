---
title: ScrollController jumpTo animateTo
date: 2026-04-07
keywords: flutter, ScrollController, jumpTo, animateTo
---
ScrollController 可以滾動頁面。<br>
SingleChildScrollView有一個屬性controller。

- 滾動到某個「位置」，使用 ScrollController，可以用jumpTo()或 animateTo()。

## jumpTo
jumpTo(位置)，因每一個元件高度是100，所以有10個元件，`10 * 100`就會跳到底部。<br>
```
_controller.jumpTo(0);  // 跳到頂部
_controller.jumpTo(100 * 10);
```

## maxScrollExtent 移動到底部
```
_controller.jumpTo(_controller.position.maxScrollExtent);
```

## 動畫效果
語法:
```
 _controller.animateTo(移動位置,
                duration: 多久時間, curve: 動畫效果);
 _controller.animateTo(_controller.position.maxScrollExtent,
                duration: Duration(seconds: 1), curve: Curves.easeOut);
```

## Duration
一秒
```
Duration(seconds: 1)
```

300毫秒
```
Duration(milliseconds: 300)
```

![img]({{site.imgurl}}/flutter/single_scroll2.png)<br>

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

class MainPage extends StatefulWidget {
  MainPage({Key? key}) : super(key: key);

  @override
  _MainPageState createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  ScrollController _controller = ScrollController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            // 使用 Stack包住 Positioned，才
            body: Stack(children: [
      SingleChildScrollView(
        controller: _controller,
        child: Column(
          children: List.generate(10, (index) {
            return Container(
              margin: EdgeInsets.only(top: 10.0),
              width: double.infinity,
              color: Colors.blue,
              height: 100,
              alignment: Alignment.center,
              child: Text("item $index"),
            );
          }),
        ),
      ),
      Positioned(
        top: 0,
        right: 0,
        child: GestureDetector(
          onTap: () {
            _controller.jumpTo(0);
          },
          child: Container(
            decoration: BoxDecoration(
              color: Colors.red,
              borderRadius: BorderRadius.circular(40),
            ),
            width: 100,
            height: 100,
            alignment: Alignment.center,
            child: Text("Top"),
          ),
        ),
      ),
      Positioned(
        bottom: 0,
        right: 0,
        child: GestureDetector(
          onTap: () {
            _controller.jumpTo(100 * 10);
          },
          child: Container(
            decoration: BoxDecoration(
              color: Colors.red,
              borderRadius: BorderRadius.circular(40),
            ),
            width: 100,
            height: 100,
            alignment: Alignment.center,
            child: Text("Bottom"),
          ),
        ),
      )
    ])));
  }
}
{% endhighlight %}