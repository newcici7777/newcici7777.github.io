---
title: Map
date: 2026-03-28
keywords: flutter, dart, Map
---
## 語法
Map是花括號，內容是key + 冒號 + value，所組合。
```
Map map = {key: value}
```
{% highlight dart linenos %}
void main() {
  Map map = {"name": "cici", "age": 18};
}
{% endhighlight %}

## 取值
使用方括號`[key]`，輸出某個key的值。<br>
{% highlight dart linenos %}
void main() {
  Map map = {"name": "cici", "age": 18};
  print(map["name"]);
}
{% endhighlight %}
```
cici
```

## forEach
{% highlight dart linenos %}
void main() {
  Map map = {"name": "cici", "age": 18};
  map.forEach((key, value) {
    print("$key:$value");
  });
}
{% endhighlight %}

## Map其它操作方法
{% highlight dart linenos %}
void main() {
  Map map = {"name": "cici", "age": 18};
  map.addAll({"address": "Taiwan"});
  print(map.containsKey("name"));
  print(map.containsValue("Taiwan"));
  map.remove("name");
  map.clear();
}
{% endhighlight %}
```
true
true
```
