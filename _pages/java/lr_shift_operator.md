---
title: 左移右移運算子
date: 2025-06-20
keywords: java, left and right shift operators
---
## 右移左移符號
```
>> 右移1位
<< 左移1位
```
左移代表進位。

右移代表退位。

## 語法
右移1位
```
10進位數字 >> 1
5 >> 1
```

右移2位
```
10進位數字 >> 2
5 >> 2
```

左移1位
```
10進位數字 << 1
5 << 1
```

左移2位
```
10進位數字 << 2
5 << 2
```

## 十進位系統左移右移

![img]({{site.imgurl}}/java/decimal.png)

右移一位除10，左移一位乘10

[十進位系統][1]。

## 二進位系統左移右移
二進位與十進位的運作方式一模一樣。

### 右移
二進位系統下，右移1位就是除2，右移2位就是除2再除2，就是除4。

![img]({{site.imgurl}}/java/binary_shift1.png)

#### 右移1位
1右移1位 = $$ 1 \div 2 = 0 $$
```
00000001
↓
右移1位
↓
00000000
```

程式碼
{% highlight java linenos %}
System.out.println(1 >> 1);  // 1右移1位
{% endhighlight %}
結果為0


15右移1位 = $$ 15 \div 2 = 7 $$，整數相除只取整數，小數位直接去掉。
```
00001111
↓
右移1位
↓
00000111
```

程式碼
{% highlight java linenos %}
System.out.println(15 >> 1);  //15右移1位
{% endhighlight %}
結果為7

#### 右移2位
右移2位就是除2再除2，就是除4，整數相除只取整數，小數位直接去掉。

15右移2位 = $$ 15 \div 2 \div 2 = 3 $$
```
00001111
↓
右移1位
↓
00000111
↓
右移1位
↓
00000011
```
程式碼
{% highlight java linenos %}
System.out.println(15 >> 2);
{% endhighlight %}
結果為3

#### 右移3位
右移3位就是除2再除2、再除2，就是除8。

15右移2位 = $$ 15 \div 2 \div 2 \div 2 = 1 $$
```
00001111
↓
右移1位
↓
00000111
↓
右移1位
↓
00000011
↓
右移1位
↓
00000001
```

程式碼
{% highlight java linenos %}
System.out.println(15 >> 3);
{% endhighlight %}
結果為1

## 左移
左移一位就是乘2，左移二位就是乘4，左移三位就是乘8。

### 左移1位
1左移1位 = $$ 1 \times 2 = 2 $$
```
00000001
↓
左移1位
↓
00000010
```

程式碼
{% highlight java linenos %}
System.out.println(1 << 1);
{% endhighlight %}
結果為2

### 左移2位
1左移2位 = $$ 1 \times 2 \times 2 = 4 $$
```
00000001
↓
左移1位
↓
00000010
↓
左移1位
↓
00000100
```

程式碼
{% highlight java linenos %}
System.out.println(1 << 2);
{% endhighlight %}
結果為4

[1]: {% link _pages/math/carry10.md %}