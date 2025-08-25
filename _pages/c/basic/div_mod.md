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
還剩下97天就放暑假了，請問還剩下幾個禮拜又幾天？<br>

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


