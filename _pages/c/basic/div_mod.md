---
title: 除法與餘數
date: 2025-05-25
keywords: c++, div , mod
---
## 整數相除，留下整數
如果希望有小數點，參與除法的值要有小數點。<br>
以下的程式碼，參與除法的值都是整數，整數相除就會去掉小數。<br>
整數相除，只保留整數，無條件去掉小數。<br>
{% highlight c++ linenos %}
int main() {
  double result = 10 / 4;
  printf("result = %.2f \n", result);
  return 0;
}
{% endhighlight %}
```
2.00
```

把參與除法的數字改成double 10.0，這樣除下來的結果就是小數。<br>
{% highlight c++ linenos %}
int main() {
  double result = 10.0 / 4;
  printf("result = %.2f \n", result);
  return 0;
}
{% endhighlight %}
```
result = 2.50 
```

## 溫度
華氏轉攝氏公式<br>
5/9 \* (華氏溫度F - 32) = 攝氏C<br>
以下結果為0.00<br>
{% highlight c++ linenos %}
int main() {
  int tempF = 100;
  double tempC = 5/9 * (tempF - 30);
  printf("tempC %.2f \n", tempC);
  return 0;
}
{% endhighlight %}
```
tempC 0.00 
```

原因在於整數相除，只會得到整數，小數無條件捨去。<br>
把參與除法的整數，改為double，運算就會提升成double在運算。<br>
{% highlight c++ linenos %}
int main() {
  int tempF = 100;
  double tempC = 5.0/9 * (tempF - 30);
  printf("tempC %.2f \n", tempC);
  return 0;
}
{% endhighlight %}
```
tempC 38.89 
```

## 餘數
餘數就是不夠除，取出剩下不夠除的數字。

1小時有3600秒，3700秒是幾小時幾分幾秒?<br>
3700秒 - 3600秒(1小時) = 剩下100秒。<br>
100秒 - 60秒(1分鐘) = 剩下40秒。<br>

### 小時 分鐘 秒 換算
把秒轉成xx小時xx分鐘xx秒
{% highlight c++ linenos %}
int main() {
  int second = 3700;
  // 1小時是60分鐘 * 60秒
  int hour = 60 * 60;
  int h = second / hour;
  int min = second % hour / 60;
  int sec = second % hour % 60;
  cout << h << "時" << min  << "分" << sec << "秒" << endl;
  return 0;
}
{% endhighlight %}
```
1時1分40秒
```

### 剩下幾個禮拜又幾天
還剩下97天就放暑假了，請問還剩下幾個禮拜又幾天？<br>

題目97天，很大，換成小一點的數字思考，9天還剩下幾個禮拜又幾天？<br>

一個禮拜7天。<br>
9天 - 7天(一個禮拜) = 2<br>

用掉1個禮拜，還剩下2天。<br>

餘數就是剩下的概念，但剩下的是有範圍，介於0到6之間，不會是7。<br>

禮拜，使用除法<br>
剩幾天，使用mod<br>

{% highlight c++ linenos %}
int main() {
  int total_day = 97;
  int week = total_day / 7;
  int day = total_day % 7;
  printf("week = %d, day = %d \n", week, day);
  return 0;
}
{% endhighlight %}
```
week = 13, day = 6 
```

## 除法 減法
以下二種是相同。
```
y = y / x;
y /= x;
```
{% highlight c++ linenos %}
int main() {
  int y = 12;
  int x = 3;
  // y = y / x;
  y /= x;
  cout << y << endl;
  return 0;
}
{% endhighlight %}
```
4
```

以下二種是相同。
```
y = y % x;
y %= x;
```
{% highlight c++ linenos %}
int main() {
  int y = 11;
  int x = 3;
  // y = y % x;
  y %= x;
  cout << y << endl;
  return 0;
}
{% endhighlight %}
```
2
```
