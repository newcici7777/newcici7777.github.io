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


