---
title: Row
date: 2026-04-01
keywords: flutter, widget, row
---
Row是水平排列，所以主軸(mainAxis)就是水平。<br>
crossAxis就是垂直。<br>

![img]({{site.imgurl}}/compose/row2.png)<br>

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


有A、B、C，三個Text()，水平排列範例
```
ABC
```

|屬性|說明|
|:----:|:---------------------|
|mainAxisAlignment|水平對齊|
|crossAxisAlignment|垂直對齊|
|children|子元素|

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: Container(
        width: 200,
        height: 200,
        color: Colors.yellow,
        child: Row(
          children: [
            Text("A"),
            Text("B"),
            Text("C"),
          ],
        ),
      ),
    ));
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/flutter/row1.png)<br>

## 對齊方式
### 垂直對齊 crossAxis
- start 上
- center 置中
- end 下

### 水平對齊 mainAxis
start 靠左對齊:
```
ABC......
```

end 靠右對齊
```
......ABC
```

spaceBetween: 左右各一個。
```
A...B...C
```

spaceAround: A與B與C，間距是3格。
```
..A...B...C..
```

spaceEvenly: 平均分配
```
...A...B...C...
``

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: Container(
        width: 200,
        height: 200,
        color: Colors.yellow,
        child: Row(
          mainAxisAlignment: MainAxisAlignment.spaceBetween,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text("A"),
            Text("B"),
            Text("C"),
          ],
        ),
      ),
    ));
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/flutter/row2.png)<br>
