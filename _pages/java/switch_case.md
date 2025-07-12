---
title: Switch case
date: 2025-07-10
keywords: Java, switch case
---
## 語法
```
switch(值) {
  case 常數1:
	程式碼
	break;
  case 常數2:
	程式碼
	break;
  case 常數3:
	程式碼
	break;
  default:
    程式碼	
}
```

![img]({{site.imgurl}}/java/switch_case.png)

## 變數與常數
只支援下方表格的基本型態與類別。

|分類|基本型態|類別|
|:----|:---------|:----------|
|整數|byte, short, int|Byte, Short, Integer|
|字元字串|char|Character, String|
|列舉||enum|

不支援小數點double,float


### 字串
{% highlight java linenos %}
String weather = "雨天";
switch (weather) {
  case "晴天":
    System.out.println("天氣好");
    break;
  case "雨天":
    System.out.println("帶雨傘");
    break;
  default:
    System.out.println("不確定什麼天氣");
}
{% endhighlight %}
```
帶雨傘
```
### 字元
常數可以為整數，因為整數與字元可以互相轉型。
{% highlight java linenos %}
char ch = 'A';
switch (ch) {
  case 65:
    System.out.println('a');
    break;
  case 66:
    System.out.println('b');
    break;
}
{% endhighlight %}
```
a
```

### 不支援小數點
不支援double與float。<br>
以下編譯錯誤。<br>
{% highlight java linenos %}
double val = 55.5;
switch (val) {
  case 55.5:
    break;
  case 66.6:
    break;
}
{% endhighlight %}

只能透過轉成整數來判斷
{% highlight java linenos %}
double score = 80.5;
if(score >= 100) {
  System.out.println("優");
}
switch ((int) score / 10) {
  case 9:
    System.out.println("優");
    break;
  case 8:
    System.out.println("甲");
    break;
  case 7:
    System.out.println("乙");
    break;
  default:
    System.out.println("丙");
}
{% endhighlight %}
```
甲
```

## default
若case條件全部不符合，會來到default。

1. default可以有，也可以沒有。
2. default不用有break，執行到default，之後就會離開switch case。

## break
若case條件都沒有break，直接「略過」下一個case條件判斷，直接執行下一個case中的程式碼。<br>
以下程式碼除了晴天有判斷外，直接「略過」下一個case條件判斷，直接執行下一個case中的程式碼。
{% highlight java linenos %}
String weather = "晴天";
switch (weather) {
  case "晴天":
    System.out.println("天氣好");
  case "雨天":
    System.out.println("帶雨傘");
  case "陰天":
    System.out.println("烏雲密布");
  default:
    System.out.println("不確定什麼天氣");
}
{% endhighlight %}
```
天氣好
帶雨傘
烏雲密布
不確定什麼天氣
```

![img]({{site.imgurl}}/java/switch_case_break2.png)

紅色線為沒有break，略過case條件判斷，直接執行case中的程式碼。<br>
![img]({{site.imgurl}}/java/switch_case_break.png)

遇到break，才會離開switch case，如果都沒有遇到break，最後會執行到default，然後離開switch case。

### 沒有break的範例
根據月份，判斷春夏秋冬。
{% highlight java linenos %}
int month = 7;
switch (month) {
  case 12:
  case 1:
  case 2:
    System.out.println("冬");
    break;
  case 3:
  case 4:
  case 5:
    System.out.println("春");
    break;
  case 6:
  case 7:
  case 8:
    System.out.println("夏");
    break;
  case 9:
  case 10:
  case 11:
    System.out.println("秋");
    break;
}
{% endhighlight %}
```
夏
```