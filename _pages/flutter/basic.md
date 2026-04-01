---
title: runApp()
date: 2026-03-31
keywords: flutter, widget, runApp()
---
## runApp(widget)
啟動App的入口runApp()。
{% highlight dart linenos %}
void main() {
  runApp(const MyApp());
}
{% endhighlight %}

語法:
```
runApp(Widget)
```

runApp()參數為Widget，flutter中，所有東西都是Widget所組成。<br>
Widget 中文是佈局元件。<br>

## MaterialApp()
MaterialApp()是Google的視覺化主題佈局元件。<br>

參數有 title(標題)、theme(主題)、home 為App的Body。
```
MaterialApp(
  title:  '標題名稱',
  theme: 視覺化主題,
  home: Scaffold()
)
```

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    title: 'Flutter Base',
    theme: ThemeData(scaffoldBackgroundColor: Colors.blue),
    home: Scaffold()
  ));
}
{% endhighlight %}

## Scaffold() 骨架

|屬性|說明|
|:----:|:---------------------|
|appBar|app標題|
|body|主要內容|
|bottomNavigationBar|底部導覽|
|backgroundColor|背景顏色|
|floatingActionButton|浮動按鈕|

{% highlight dart linenos %}
void main() {
  runApp(MaterialApp(
    title: 'Flutter Base',
    theme: ThemeData(scaffoldBackgroundColor: Colors.blue),
    home: Scaffold(
      appBar: AppBar(
        title: Text('AppBar'),
      ),
      body: Container(
        child: Center(
          child: Text('Body'),
        ),
      ),
      bottomNavigationBar: Container(
        height: 50,
        child: Center(
          child: Text('Bottom Navigation Bar'),
        ),
      ),
    ),
  ));
}
{% endhighlight %}

![img]({{site.imgurl}}/flutter/scaffold.png)<br>

## StatelessWidget
不會改變的靜態頁面，繼承StatelessWidget，並實作build()方法。<br>

快速鍵，輸入stateless<br>
![img]({{site.imgurl}}/flutter/stateless.png)<br>

把上面MaterialApp(...)全複製到build()方法中的return 後面。<br>
在runApp()參數設成MainPage(自訂類別名)。<br>
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
      title: 'Flutter Base',
      theme: ThemeData(scaffoldBackgroundColor: Colors.blue),
      home: Scaffold(
        appBar: AppBar(
          title: Text('AppBar'),
        ),
        body: Container(
          child: Center(
            child: Text('Body'),
          ),
        ),
        bottomNavigationBar: Container(
          color: Colors.amber,
          height: 50,
          child: Center(
            child: Text('Bottom Navigation Bar'),
          ),
        ),
      ),
    );
  }
}
{% endhighlight %}

## StatefulWidget
若畫面有Click按鈕，有互動，會影嚮畫面變化，要繼承StatefulWidget。<br>

快速鍵，輸入stateful<br>
![img]({{site.imgurl}}/flutter/stateful.png)<br>

建立二個類別，分別繼承StatefulWidget、State，步驟如下:<br>
1. 供外部讀取，繼承StatefulWidget
2. 供內部私有使用，繼承 State<供外部讀取的類別名>
3. 把會變動的內容，寫在_MainPageState類別，build()方法。

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
  int count = 0;
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Base',
      theme: ThemeData(scaffoldBackgroundColor: Colors.blue),
      home: Scaffold(
          appBar: AppBar(
            title: Text('AppBar'),
          ),
          body: Container(
            child: Row(
              children: [
                TextButton(
                  onPressed: () {
                    setState(() {
                      count++;
                    });
                  },
                  child: Text("Add"),
                ),
                Text(count.toString()),
                TextButton(
                  onPressed: () {
                    setState(() {
                      count--;
                    });
                  },
                  child: Text("Subtract"),
                ),
              ],
            ),
          )),
    );
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/flutter/stateful1.png)<br>
![img]({{site.imgurl}}/flutter/stateful2.png)<br>

## Container

|分類|屬性|說明|
|:----:|:----:|:---------------------|
|對齊|alignment|center,topLeft(左上角)|
|寬高|width,height|整數、double.infinity為自適應螢幕寬/高|
|間距|margin,padding |margin物體與外面間距,padding物體內間距|
|顏色|color|背景顏色|
|裝飾|color,decoration|背景顏色、decoration圓角、border顏色|
|旋轉|transform|旋轉|
|子widget元件|child|只能「一個」子佈局元件，不能多個|

佈
{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

lass MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: Container(
        // 旋轉
        transform: Matrix4.rotationZ(0.5),
        // 與外圍間距
        margin: EdgeInsets.all(20),
        // 內部間距
        padding: EdgeInsets.all(20),
        // 對齊
        alignment: Alignment.topLeft,
        width: 200,
        height: 200,
        // 裝飾，邊框用decoration
        decoration: BoxDecoration(
          // 背景顏色
          color: Colors.blue,
          // 圓角
          borderRadius: BorderRadius.circular(15),
          // 邊框
          border: Border.all(width: 5, color: Colors.red),
        ),
        // 只能一個子元件
        child: Text("Hello World"),
      ),
    ));
  }
}

{% endhighlight %}

![img]({{site.imgurl}}/flutter/container1.png)<br>
