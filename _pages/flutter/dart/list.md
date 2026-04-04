---
title: List
date: 2026-03-28
keywords: flutter, dart, List
---
## 語法
元素以方括號`[]`包住，逗號區隔每個元素。
{% highlight dart linenos %}
  List list = [1, 2, 3, 4, 5];
  print(list);
{% endhighlight %}
```
[1, 2, 3, 4, 5]
```

## List相關方法
{% highlight dart linenos %}
  List list = [1, 2, 3, 4, 5];
  // 新增
  list.add(6);
  // 刪除
  list.remove(3);
  // 移除最後一個
  list.removeLast();
{% endhighlight %}

## removeRange(start, end)
Range會有二個參數，分別為 start 與 end。<br>
Range介於 start <= range < end，下面的程式碼會刪除 index 大於等於 0，index 小於(不包含) 2。<br>
{% highlight dart linenos %}
  List list = [1, 2, 3, 4, 5];
  list.removeRange(0, 2);
  print(list);
{% endhighlight %}

## forEach
{% highlight dart linenos %}
  List list = [1, 2, 3, 4, 5];
  list.forEach((element) {
    print(element);
  });
{% endhighlight %}
```
1
2
3
4
5
```

{% highlight dart linenos %}
void main() {
  List list = [1, 3, 3, 3, 5];
  print(list.length);
  print(list.last);
  print(list.first);
  print(list.isEmpty);
}
{% endhighlight %}
```
5
5
1
false
```
## addAll
{% highlight dart linenos %}
void main() {
  List list = [1, 3, 3, 3, 5];
  list.addAll([6, 7, 8]);
  print(list);
}
{% endhighlight %}
```
[1, 3, 3, 3, 5, 6, 7, 8]
```

## every
every 傳回值是true或false，every的條件，是判斷List中，是否有符合條件的元素，若有傳回 true 或 false 。<br>

語法
```
bool result = list.every((element) {
    return 條件
  });
```

{% highlight dart linenos %}
void main() {
  List list = [1, 2, 3, 4, 5];
  bool isContain3 = list.every((element) {
    return element == 3;
  });
  print(isContain3);
}
{% endhighlight %}

## where
把符合條件的元素傳回，需要搭配toList()。<br>

語法
```
List result = list.where((element) {
    return 條件
  });
```

{% highlight dart linenos %}
void main() {
  List list = [1, 3, 3, 3, 5];
  List result = list.where((element) {
    return element == 3;
  }).toList();
  print(result);
}
{% endhighlight %}
```
[3, 3, 3]
```

element 為dynamic類別，若要做String 方法操作，先把它轉型成String，使用String.startsWith() 方法 。
{% highlight dart linenos %}
void main() {
  List list = ["Hello", "Hi", "Mary", "Bill", "5"];
  List result = list.where((element) {
    return element.toString().startsWith("H");
  }).toList();
  print(result);
}
{% endhighlight %}
```
[Hello, Hi]
```

## List.generate()
回傳值類型是`List<E>`，E是看return 傳回的類型。<br>

語法
```
List.generate(數量, (index) {
  return 要產生的東西
})
```
