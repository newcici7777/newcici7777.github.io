---
title: PageView
date: 2026-04-06
keywords: flutter, CustomScrollView
---
PageController為切換頁面。<br>
使用以下語法換頁:<br>
```
_pageController.animateToPage()
_pageController.jumpTo()
```

使用`_currentPage`變數記錄目前的頁面。<br>
```
int _currentPage = 0;
```

使用setState()，修改`_currentPage`變數:<br>
```
setState(() {
  _currentPage = index;
});
```

![img]({{site.imgurl}}/flutter/pageview.png)<br>

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
  // 目前頁數
  int _currentPage = 0;
  // controller
  PageController _pageController = PageController(initialPage: 0);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: Stack(children: [
        // 輪播圖
        Container(
          height: 100,
          child: PageView.builder(
            // controller 可以jumpTo到某個頁面
            controller: _pageController,
            itemCount: 10,
            itemBuilder: (context, index) {
              return Container(
                height: 100,
                alignment: Alignment.center,
                color: Colors.yellow,
                child: Text('Category $index'),
              );
            },
          ),
        ),
        // 圓點
        Positioned(
          // 靠底 對齊
          bottom: 0,
          // left 0 , right 0 為左右拉伸
          left: 0,
          right: 0,
          child: Container(
              height: 20,
              child: Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: List.generate(10, (index) {
                  return GestureDetector(
                      // 按一下事件
                      onTap: () {
                        // 換頁
                        _pageController.animateToPage(index,
                            duration: Duration(milliseconds: 300),
                            curve: Curves.easeInOut);
                        // 修改_currentPage狀態
                        setState(() {
                          _currentPage = index;
                        });
                      },
                      child: Container(
                        width: 10,
                        height: 10,
                        // 左右間隔
                        margin: EdgeInsets.symmetric(horizontal: 5),
                        // 圓點樣式
                        decoration: BoxDecoration(
                          // 若為選擇頁面，圓點則為紅色。
                          color:
                              _currentPage == index ? Colors.red : Colors.blue,
                          borderRadius: BorderRadius.circular(5),
                        ),
                      ));
                }),
              )),
        ),
      ]),
    ));
  }
}
{% endhighlight %}