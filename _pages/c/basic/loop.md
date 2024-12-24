---
title: 迴圈
date: 2024-07-25
keywords: c++, loop
---

## while

### while條件判斷式
```
while(條件判斷式) {
	迴圈主體(the body of an iteration statement)
}
```

條件判斷式為True，就進入迴圈主體，執行完迴圈主體，再回到條件判斷式，若為True，再進入迴圈主體，直到條件判斷式為False，就離開迴圈。

### while初始運算式

while的初始運算式是在while之前先定義好。

```
	int i = 0;
```

### while遞增運算式

若條件判斷式一直為True，就會成為無窮迴圈，所以需要有改變條件判斷式的方法，讓條件變為False，離開迴圈。使用遞增遞減運算式改變條件判斷式，遞增運算式定義在while迴圈中。

```
while (i <= 100) {
	i++;
}
```

### 完整程式碼

{% highlight c++ linenos %}
int main() {
	//初始運算式
  int i = 0;
  //條件判斷式
  while (i <= 10) {
    cout << i << endl;
    //遞增運算式
    i++;
  }
  return 0;
}
{% endhighlight %}

也可以簡化成一行。

{% highlight c++ linenos %}
int main() {
  int i = 0;
  while (true)
    cout << i++ << endl;
  return 0;
}
{% endhighlight %}

```
0
1
2
3
4
5
6
7
8
9
10
```

### while無窮迴圈1

若條件判斷式一直為True，就會成為無窮迴圈，無法離開循環。

若上述範例缺少遞增運算式，就會變成無窮迴圈一直執行迴圈主體無法停止。

{% highlight c++ linenos %}
while (i <= 100) {
	//無窮迴圈
}
{% endhighlight %}

### while無窮迴圈2

若條件判斷式為true，就會成為無窮迴圈。

{% highlight c++ linenos %}
while (true) {
	//無窮迴圈
}
{% endhighlight %}

### break跳出迴圈

若符合特定條件，就離開迴圈。

以下程式碼是若sum >= 5000就離開迴圈。
{% highlight c++ linenos %}
int main() {
  int sum = 0;
  while (true) {
    int input = 0;
    cout << "請輸入數字:"; cin >> input;
    sum += input;
    if (sum >= 5000) {
      break;
    }
  }
  cout << "sum = " << sum << endl;
  return 0;
}
{% endhighlight %}
```
請輸入數字:1001
請輸入數字:1002
請輸入數字:1003
請輸入數字:1004
請輸入數字:1005
sum = 5015
```

### continue 回到迴圈的條件判斷式

若符合特定條件，就直接回到迴圈的條件判斷式，後面的程式碼就不執行。

以下程式碼是印出1-50是5的倍數。
若不是5的倍數，直接回到迴圈的條件判斷式，
{% highlight c++ linenos %}
int main() {
  int i = 1;
  while (i <= 50) {
    if ((i % 5) != 0) {
      i++;
      continue;
    }
    cout << i << endl;
    i++;
  }
  return 0;
}
{% endhighlight %}
```
5
10
15
20
25
30
35
40
45
50
```

## for

for是while的變形。

`條件判斷式`與`初始運算式`與`遞增運算式`寫在同一行。

```
for (初始運算式;條件判斷式;遞增運算式) {

}
```

以下程式碼印出5的倍數，遞增運算式是i = i + 5

{% highlight c++ linenos %}
int main() {
  for (int i = 0; i <= 50; i = i + 5) {
    cout << i << endl;
  }
  return 0;
}
{% endhighlight %}

### for初始運算式與遞增運算式

初始運算式可以寫在for之前。

{% highlight c++ linenos %}
int main() {
	//i寫在for之前
  int i = 0;
  for (; i <= 50; i = i + 5) {
    cout << i << endl;
  }
  return 0;
}
{% endhighlight %}

`初始運算式`與`遞增運算式`可以有多個，用逗號分開，但條件判斷式只能一個。

{% highlight c++ linenos %}
int main() {
  for (int i = 0, j = 50; i <= 50; i = i + 5, j = j - 5) {
    cout << "i = " << i << endl;
    cout << "j = " << j << endl;
  }
  return 0;
}
{% endhighlight %}

`遞增運算式`可以寫在迴圈主體中。

{% highlight c++ linenos %}
int main() {
  //初始運算式 定義在for之前
  int i = 0, j = 50;
  for (; i <= 50;) {
    cout << "i = " << i << ", ";
    cout << "j = " << j << endl;
    //遞增運算式寫在迴圈主體
    i = i + 5;
    j = j - 5;
  }
  return 0;
}
{% endhighlight %}
```
i = 0, j = 50
i = 5, j = 45
i = 10, j = 40
i = 15, j = 35
i = 20, j = 30
i = 25, j = 25
i = 30, j = 20
i = 35, j = 15
i = 40, j = 10
i = 45, j = 5
i = 50, j = 0
```
### for無窮迴圈

`條件判斷式`與`初始運算式`與`遞增運算式`都不寫的狀況下是無窮迴圈，需要使用break離開迴圈。
{% highlight c++ linenos %}
int main() {
  int sum = 0;
  for (;;) {
    int input = 0;
    cout << "請輸入數字:"; cin >> input;
    sum+=input;
    if (sum >= 5000) break;
  }
  cout << "sum = " << sum << endl;
  return 0;
}
{% endhighlight %}
```
請輸入數字:1000
請輸入數字:1000
請輸入數字:1000
請輸入數字:1001
請輸入數字:1002
sum = 5003
```
