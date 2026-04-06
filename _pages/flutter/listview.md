---
title: ListView
date: 2026-04-04
keywords: flutter, ListView
---
## 建構方法
ListView 提供多種 提供多種建構方法:

|建構方法|說明|
|:--------|:--------------------|
|ListView()|不是Lazy Loading，一次建立所有列表子元素|
|ListView.builder()|Lazy Loading|
|ListView.separated()|Lazy Loading|

### Lazy Loading

只有滑動到可見區域，才會建立子元件，之前已滑過沒在可見區域就會被記憶體回收，釋放記憶體空間。

## ListView.builder()
### itemCount 要建立的列數
語法
```
itemCount: 列數
itemCount: 100
```

### itemBuilder 建立子元件
語法
```
itemBuilder: (BuildContext context, int index) {
  // 建立每一列子元件
}
```

![img]({{site.imgurl}}/flutter/listview2.png)<br>

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
            body: ListView.builder(
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

## ListView.separated()
跟ListView.builder() 一樣有 itemBuilder、itemCount，只是多了separatorBuilder。<br>

### separatorBuilder 分隔線
語法
```
separatorBuilder: (BuildContext context, int index) {
  // 建立分隔線子元件
}
```

### itemBuilder 建立子元件
語法
```
itemBuilder: (BuildContext context, int index) {
  // 建立每一列子元件
}
```

![img]({{site.imgurl}}/flutter/listview1.png)<br>

{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  ScrollController _controller = ScrollController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: ListView.separated(
                // 每個元件樣式
                itemBuilder: (BuildContext context, int index) {
                  return Container(
                    height: 100,
                    color: Colors.blue,
                    alignment: Alignment.center,
                    child: Text("item $index"),
                  );
                },
                // 分隔線樣式
                separatorBuilder: (BuildContext context, int index) {
                  return Container(
                    height: 10,
                    color: Colors.yellow,
                  );
                },
                // 產生數量
                itemCount: 100)));
  }
}
{% endhighlight %}