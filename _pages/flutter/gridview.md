---
title: GridView
date: 2026-04-04
keywords: flutter, GridView
---
## 建構方法
- GridView.count() 固定欄數或列數
- GridView.extent() 子元素最大寬度或高度
- GridView.builder() 搭配 gridDelegate

## GridView.count()
使用count()建構方法，建立Widget。<br>
```
GridView.count(...)
```

### scrollDirection 滾動的方向
注意！滾動的方法預設是垂直(Axis.vertical)

|屬性|說明|
|:----|:-------------|
|Axis.vertical(預設)|垂直滾動|
|Axis.horizontal|水平滾動|

### scrollDirection: Axis.vertical
#### crossAxisCount 交叉軸數量
滾動方向(scrollDirection)是垂直(Axis.vertical)。<br>
mainAxis 主軸是 vertical 垂直，crossAxis 交叉軸 就是水平。<br>

```
	mainAxis
		|
		|
		|
		|
------------------> crossAxis
		|
		|
		|
		|
```

crossAxis 交叉軸是水平，假設crossAxisCount 為 3 ，會呈現「每列只有三個元素」。<br>

```
x x x  每列只有三個元素
x x x
x x x
x x x
x x x
x x x
```

![img]({{site.imgurl}}/flutter/gridview1.png)<br>

#### Axis.vertical 間隔 Spacing
mainAxisSpacing 為 Axis.vertical。<br>
mainAxisSpacing 上下的間隔大小。<br>
crossAxisSpacing 左右的間隔大小。<br>

### scrollDirection: Axis.horizontal
滾動方向(scrollDirection)是水平(Axis.horizontal)。<br>
mainAxis 主軸是 horizontal 水平，crossAxis 交叉軸 就是垂直。<br>
```
  crossAxis
    |
    |
    |
    |
------------------> mainAxis
    |
    |
    |
    |
```

crossAxis 交叉軸是垂直，假設crossAxisCount 為 3 ，就固定只有三「列」。<br>

```
x x x x x x
x x x x x x
x x x x x x
```

![img]({{site.imgurl}}/flutter/gridview2.png)<br>

#### Axis.horizontal 間隔 Spacing
mainAxisSpacing 為 Axis.horizontal<br>
mainAxisSpacing 左右的間隔大小。<br>
crossAxisSpacing 上下的間隔大小。<br>

### 程式碼
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
            body: GridView.count(
      // 滾動方向
      scrollDirection: Axis.vertical,
      // 根據crossAxis 固定行數或列數
      crossAxisCount: 3,
      // 間隔 大小
      mainAxisSpacing: 10,
      crossAxisSpacing: 10,
      // 產生100個元素
      children: List.generate(100, (index) {
        return Container(
          height: 100,
          color: Colors.blue,
          alignment: Alignment.center,
          child: Text("item $index"),
        );
      }),
    )));
  }
}

{% endhighlight %}

## GridView.extent()
固定「寬度」或「高度」的佈局元件。<br>
### scrollDirection:Axis.horizontal
每一個子元素的最大「寬」度。
```
maxCrossAxisExtent: 100,
```

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
            body: GridView.extent(
      scrollDirection: Axis.horizontal,
      // 每个item的最大宽度
      // 100
      maxCrossAxisExtent: 100,
      mainAxisSpacing: 10,
      crossAxisSpacing: 10,
      children: List.generate(10, (index) {
        return Container(
          height: 100,
          color: Colors.blue,
          alignment: Alignment.center,
          child: Text("item $index"),
        );
      }),
    )));
  }
}

{% endhighlight %}

### scrollDirection:Axis.vertical
每一個元素的最大「高」度。
```
maxCrossAxisExtent: 100,
```

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
            body: GridView.extent(
      scrollDirection: Axis.vertical,
      // 每一個元素的最大「高」度。
      maxCrossAxisExtent: 100,
      mainAxisSpacing: 10,
      crossAxisSpacing: 10,
      children: List.generate(10, (index) {
        return Container(
          height: 100,
          color: Colors.blue,
          alignment: Alignment.center,
          child: Text("item $index"),
        );
      }),
    )));
  }
}
{% endhighlight %}

## GridView.builder()
屬於Lazy Loading，只有滑動到可見區域，才會建立子元件，之前已滑過沒在可見區域就會被記憶體回收，釋放記憶體空間。

### itemCount 要建立的數量
語法
```
itemCount: 列數
itemCount: 10
```

### itemBuilder 建立子元件
語法
```
itemBuilder: (BuildContext context, int index) {
  // 建立每一列子元件
}
```

### gridDelegate 佈局組件委托
- SliverGridDelegateWithFixedCrossAxisCount 固定列數或欄數
- SliverGridDelegateWithMaxCrossAxisExtent 固定每個元素的寬度或高度

### SliverGridDelegateWithFixedCrossAxisCount
固定列數或欄數
#### scrollDirection: Axis.vertical
GridView 預設 scrollDirection 滾動的方向是垂直(Axis.vertical)

固定欄數為2欄。<br>
![img]({{site.imgurl}}/flutter/gridview3.png)<br>

{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  ScrollController _controller = ScrollController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: GridView.builder(
                scrollDirection: Axis.vertical,
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 2,
                    mainAxisSpacing: 10,
                    crossAxisSpacing: 10),
                itemBuilder: (context, index) {
                  return Container(
                    height: 100,
                    color: Colors.blue,
                    alignment: Alignment.center,
                    child: Text("item $index"),
                  );
                },
                itemCount: 10)));
  }
}
{% endhighlight %}

#### scrollDirection: Axis.horizontal
scrollDirection 滾動的方向是水平(Axis.horizontal)

固定列數為2列。<br>
![img]({{site.imgurl}}/flutter/gridview4.png)<br>

{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  ScrollController _controller = ScrollController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: GridView.builder(
                scrollDirection: Axis.horizontal,
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 2,
                    mainAxisSpacing: 10,
                    crossAxisSpacing: 10),
                itemBuilder: (context, index) {
                  return Container(
                    height: 100,
                    color: Colors.blue,
                    alignment: Alignment.center,
                    child: Text("item $index"),
                  );
                },
                itemCount: 10)));
  }
}
{% endhighlight %}

### SliverGridDelegateWithMaxCrossAxisExtent
根據滾動方向scrollDirection，固定每個元素的寬度或高度。<br>

![img]({{site.imgurl}}/flutter/gridview5.png)<br>

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
            body: GridView.builder(
      scrollDirection: Axis.horizontal,
      gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
          maxCrossAxisExtent: 100, mainAxisSpacing: 10, crossAxisSpacing: 10),
      itemBuilder: (context, index) {
        return Container(
          height: 100,
          color: Colors.blue,
          alignment: Alignment.center,
          child: Text("item $index"),
        );
      },
      itemCount: 10,
    )));
  }
}
{% endhighlight %}

#### childAspectRatio 寬高比
根據滾動方向scrollDirection，以下比例會呈現不同形狀。

- childAspectRatio: 1 是正方形
- childAspectRatio: 2 長方形
- childAspectRatio: 0.5 長方形

![img]({{site.imgurl}}/flutter/gridview6.png)<br>

![img]({{site.imgurl}}/flutter/gridview7.png)<br>

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
            body: GridView.builder(
      scrollDirection: Axis.horizontal,
      gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
          maxCrossAxisExtent: 100,
          mainAxisSpacing: 10,
          crossAxisSpacing: 10,
          childAspectRatio: 0.5),
      itemBuilder: (context, index) {
        return Container(
          height: 100,
          color: Colors.blue,
          alignment: Alignment.center,
          child: Text("item $index"),
        );
      },
      itemCount: 10,
    )));
  }
}

{% endhighlight %}