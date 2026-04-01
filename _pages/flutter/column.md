---
title: Column
date: 2026-03-31
keywords: flutter, widget, column
---
Column是垂直排列，所以主軸(mainAxis)就是垂直。<br>
crossAxis就是水平。<br>

![img]({{site.imgurl}}/compose/column1.png)<br>

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

有A、B、C，三個Text()，垂直排列範例:
```
A
B
C
```

|屬性|說明|
|:----:|:---------------------|
|mainAxisAlignment 主軸|垂直對齊|
|crossAxisAlignment 交叉軸|水平對齊|
|children|子元素|

以下程式碼，文字垂直方向排列，預設為置中。<br>
```
	A
	B
	C
```
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
        child: Column(
          children: [
            Text("A"),
            Text("B"),
          ],
        ),
      ),
    ));
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/flutter/column1.png)<br>

## 對齊方式
### 垂直對齊 mainAxis
start: 上
```
A
B
C
.
.
.
```

end: 下
```
.
.
.
A
B
C
```

spaceBetween: 上下各一個元素，中間置中。
```
A
.
.
B
.
.
C
```

spaceAround: item1與item2與item3之間有三個空格。
```
·
·
[item1]
·
·
·
[item2]
·
·
·
[item3]
·
·
```

spaceEvenly: 空間平均分配。
```
·
·
[item1]
·
·
[item2]
·
·
[item3]
·
·
```

### 水平對齊 crossAxis

- start 靠左
- center 置中
- end 靠右

### 程式碼
以下程式碼，垂直分散對齊，水平靠左對齊。<br>
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
        child: Column(
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

![img]({{site.imgurl}}/flutter/column2.png)<br>