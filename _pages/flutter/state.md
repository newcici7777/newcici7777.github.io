---
title: StatelessWidget, StatefulWidget, setState()
date: 2026-03-31
keywords: flutter, widget, StatelessWidget, StatefulWidget, setState
---
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

