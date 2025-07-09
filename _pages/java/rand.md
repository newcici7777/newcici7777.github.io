---
title: 亂數
date: 2025-06-11
keywords: java, rand
---
## Math.random()
預設產生0到0.999999(不包含1)之間的亂數，0 \<= 數字 \<= 0.999999，不包含整數1。
{% highlight java linenos %}
// 產生10個亂數
for (int i = 0; i < 10; i++) {
  System.out.println(Math.random());
}
{% endhighlight %}
```
0.6626947146950831
0.8865498402906333
0.9527716033115488
0.4059555335297955
0.24287815701814686
0.10780772812143591
0.005724639017029354
0.41796994366817686
0.8392257800190969
0.736425852792078
```
## random()原始碼傳回值是double
random()傳回值是double
{% highlight java linenos %}
public static double random() {
    return RandomNumberGeneratorHolder.randomNumberGenerator.nextDouble();
}
{% endhighlight %}

## 強制轉型int
- [強制轉型][1]

需要使用圓括號包住\(計算公式\)，強制轉型才會把整個\(計算公式\)的結果轉成int。
{% highlight java linenos %}
// 產生10個亂數
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random()));
}
{% endhighlight %}

## 產生亂數
### 產生0到99亂數
預設產生0到0.999999(不包含1)之間的亂數，把產生的數字乘上100，就會產生0至99.999999的亂數，不包含100。
{% highlight java linenos %}
// 產生10個亂數
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random() * 100));
}
{% endhighlight %}
```
84
56
74
95
12
9
82
53
0
61
```
### 產生0到100亂數
把100 \+ 1 = 101，產生預設產生0到0.999999(不包含1)之間的亂數，把產生的數字乘上100，就會產生0至100.999999的亂數，不包含101。
{% highlight java linenos %}
// 產生10個亂數
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random() * 101));
}
{% endhighlight %}

### 產生0到6的亂數。
以下程式碼為何要6 \+ 1 ?<br>
因為亂數產生的是0到 5.999999，產生的範圍是0 \<= x \<= 5.999999，不會等於6，所以要加1，加1之後的範圍是0到 6.9999999，0 \<= x \<= 6.999999，最大不會超過7。<br>

因為random傳回值是double，要使用int強制轉型，int強制轉型只會去掉小數點，不會四捨五入。
{% highlight java linenos %}
// 產生10個亂數
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random() * (6 + 1)));
}
{% endhighlight %}
```
3
5
3
5
5
4
1
2
0
4
```

### 產生10個1到100亂數
注意！是「1」到「100」，不是0到100，跟之前的不一樣。<br>

1.先取得0到99的double。<br>
亂數產生的是0到 99.999999，產生的範圍是0 \<= x \<= 99.999999，不會等於100<br>
{% highlight java linenos %}
// 產生10個亂數
for (int i = 0; i < 10; i++) {
  System.out.println(Math.random() * 100);
}
{% endhighlight %}
```
18.117136224378395
69.9176833968274
83.23955274354016
30.601213720220876
74.22605408548534
70.96165488922483
4.101315350235257
28.630693528715057
53.496945037049684
72.62665129052877
```

2.去掉小數點，把double強制轉成int。<br>
需要使用圓括號包住\(計算公式\)，強制轉型才會把整個\(計算公式\)的結果轉成int。<br>
{% highlight java linenos %}
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random() * 100));
}
{% endhighlight %}
```
2
18
61
35
36
22
18
71
52
21
```

3.因為求出的數字範圍是0 \<= x \<= 99，不會等於100。<br>
只要加上1，範圍就會在「1」 \<= x \<= 100，1到100之間。<br>
{% highlight java linenos %}
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random() * 100) + 1);
}
{% endhighlight %}
```
13
65
100
29
53
65
6
36
16
78
```

## 產生2到7之間的亂數
原本的Math.random()只會產生0到x的數字。

1.要產生2到7的亂數，要先產生0到5的亂數，再加上2，才會符合2到7的亂數。<br>
```
7 - 2 = 5
```
以下產生0到4.99999的亂數，不包含5。
{% highlight java linenos %}
Math.random() * (7 - 2)
{% endhighlight %}

2.但0到5的亂數，只會產生0到4.99999的亂數，所以要再 \+ 1，這樣才會產生0到5.999999的亂數，不包含6。<br>
```
5 + 1 = 6
```
{% highlight java linenos %}
Math.random() * (7 - 2 + 1)
{% endhighlight %}


3.使用強制轉型int，無條件去掉小數點。<br>
需要使用圓括號包住\(計算公式\)，強制轉型才會把整個\(計算公式\)的結果轉成int。<br>
以下程式碼產生0到5的整數亂數。<br>
{% highlight java linenos %}
(int)(Math.random() * (7 - 2 + 1))
{% endhighlight %}

4.題目要求產生2到7的亂數。<br>
把求出0到5的任意一個亂數，加上2，亂數範圍就會在2到7。
{% highlight java linenos %}
(int)(Math.random() * (7 - 2 + 1)) + 2
{% endhighlight %}

程式碼
{% highlight java linenos %}
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random() * (7 - 2 + 1)) + 2);
}
{% endhighlight %}
```
3
3
3
7
7
2
3
3
2
3
```

[1]: {% link _pages/java/number.md %}#強制轉型
