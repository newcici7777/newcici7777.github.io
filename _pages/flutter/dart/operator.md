---
title: Operator
date: 2026-03-30
keywords: flutter, dart, operator
---
## 除法
傳回值類型為 double ，有小數點。<br>
使用runtimeType屬性，查看變數類型。<br>
{% highlight dart linenos %}
void main() {
  int d1 = 9;
  var res = d1 / 5;
  print(res.runtimeType);
  print(res);
}
{% endhighlight %}
```
double
1.8
```

除法要用double 類型。
{% highlight dart linenos %}
void main() {
  int d1 = 9;
  double res = d1 / 5;
  print(res.runtimeType);
  print(res);
}
{% endhighlight %}

## 整除 `~/`
傳回值類型為 int ，無條件去掉小數點。
{% highlight python linenos %}
void main() {
  int d1 = 9;
  var res = d1 ~/ 5;
  print(res.runtimeType);
  print(res);
}
{% endhighlight %}
```
int
1
```

整除類型要用int。
{% highlight python linenos %}
void main() {
  int d1 = 9;
  int res = d1 ~/ 5;
  print(res.runtimeType);
  print(res);
}
{% endhighlight %}


## 邏輯運算子

|邏輯運算子|說明|
|:---:|:--------|
|&&| 二邊都為true則true|
|`||`|有一個為true則true|
|`!`|相反|

左右二邊運算元都要是bool 類型。<br>
以下編譯(語法)錯誤。<br>
{% highlight dart linenos %}
void main() {
  int d1 = 1;
  int d2 = 0;
  if (d1 && d2) {
    print("d1 && d2 is true");
  }
  if(d1) {
    print("true");
  }
  if(d2) {
    print("false");
  }
}
{% endhighlight %}

C++ 與 Python 可以接受任何類型做 and / or 邏輯運算，但 dart 只接受bool 進行邏輯運算。<br>

以下是修改後，dart正確程式碼。<br>
{% highlight dart linenos %}
void main() {
  bool b1 = true;
  bool b2 = false;
  if (b1 && b2) {
    print("b1 && b2 is true");
  }
  if (b1) {
    print("true");
  }
  if (b2) {
    print("false");
  }
}
{% endhighlight %}

## 三元運算子
語法
```
條件 ? 結果1 : 結果2;
```

{% highlight dart linenos %}
void main() {
  int score = 60;
  print(score >= 60 ? "及格" : "不及格");
}
{% endhighlight %}