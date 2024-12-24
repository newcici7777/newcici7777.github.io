---
title: 亂數
date: 2024-09-09
keywords: c++, rand
---

## rand()

rand() std函式庫，取得亂數，但只生成一次，每次產生的亂數不會有重覆。

以下程式碼，不管執行幾次，產生的亂數都一樣，但10個數字都不相同。

{% highlight c++ linenos %}
int main() {
  //產生10個亂數
  for (int i = 0; i < 10; i++) {
    cout << rand() << endl;
  }
  return 0;
}
{% endhighlight %}

```
16807
282475249
1622650073
984943658
1144108930
470211272
101027544
1457850878
1458777923
2007237709
```

## srand()

srand()，產生亂數種子，根據種子不同，會產生不同結果。

但亂數種子若都固定相同，每次執行產生相同結果。

{% highlight c++ linenos %}
int main() {
  srand(1);
  //產生10個亂數
  for (int i = 0; i < 10; i++) {
    cout << rand() << endl;
  }
  return 0;
}
{% endhighlight %}

```
16807
282475249
1622650073
984943658
1144108930
470211272
101027544
1457850878
1458777923
2007237709
```

{% highlight c++ linenos %}
int main() {
  srand(2);
  //產生10個亂數
  for (int i = 0; i < 10; i++) {
    cout << rand() << endl;
  }
  return 0;
}
{% endhighlight %}

```
33614
564950498
1097816499
1969887316
140734213
940422544
202055088
768218109
770072199
1866991771
```

## srand(time(0))
time(0) std函式，取得從1970-01-01累積到現在的秒數，也就是現在時間。

把time(0)作為srand()的參數，確保每次執行亂數都不會相同。

{% highlight c++ linenos %}
int main() {
  srand(time(0));
  cout << "time(0)=" << time(0) << endl;
  //產生10個亂數
  for (int i = 0; i < 10; i++) {
    cout << rand() << endl;
  }
  return 0;
}
{% endhighlight %}

```
time(0)=1726015663
936144365
1313144633
332406612
1152962037
1088008978
343639041
957835304
800536416
630495257
1049470101
```

## 產生一定範圍的隨機數字

### 產生0~20之間的亂數

假設產生0~20之間的數字，直接把亂數 % 20

{% highlight c++ linenos %}
int main() {
  srand(time(0));
  //產生10個亂數
  for (int i = 0; i < 10; i++) {
    cout << rand() % 20 << endl;
  }
  return 0;
}
{% endhighlight %}

```
10
7
14
1
17
13
10
12
8
8
```

### 產生50~70之間的亂數

把上一個範例0~20之間的隨機數字+50，就可以產生50~70之間的亂數。

50就是最小的數字

{% highlight c++ linenos %}
rand() % 20 + 50
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
int main() {
  srand(time(0));
  //產生10個亂數
  for (int i = 0; i < 10; i++) {
    cout << rand() % 20 + 50 << endl;
  }
  return 0;
}
{% endhighlight %}

```
58
55
62
52
61
60
64
65
51
60
```

### 亂數取餘數會重覆

rand()函式產生的亂數是不會重覆。
但對rand() % 20，取餘數亂數就會重覆。

{% highlight c++ linenos %}
int main() {
  srand(time(0));
  //產生10個亂數
  for (int i = 0; i < 10; i++) {
    int val = rand();
    cout << "rand = " << val;
    cout << " , rand % 20 = " << val % 20 << endl;
  }
  return 0;
}
{% endhighlight %}

```
rand = 976027376 , rand % 20 = 16
rand = 1612012646 , rand % 20 = 6
rand = 442850770 , rand % 20 = 10
rand = 1962054535 , rand % 20 = 15
rand = 1639170060 , rand % 20 = 0
rand = 1610974704 , rand % 20 = 4
rand = 178028752 , rand % 20 = 12
rand = 684514593 , rand % 20 = 13
rand = 566867572 , rand % 20 = 12
rand = 1105824512 , rand % 20 = 12
```


