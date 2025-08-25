---
title: char字元
date: 2024-04-18
keywords: c++, char
---
## 整數
整數包含`char`, bool, short, unsinged short, int, unsinged int, long, unsinged long

## char
char是正整數，雖然顯示是字元，但實際存放在記憶體位置的資料型態是整數。

字元對映的ASCII碼。<https://zh.wikipedia.org/zh-tw/ASCII>

若輸入單引號\'\'包住的字元，編譯器會自動把單引號包住的字元轉成整數。

比如\'a\'，記憶體位址存放的十進位值是97，二進位是01100001  

|整數型態|占用Byte數量|數值範圍|格式化|輸出格式|
|:---:|:---:|:---:|:---:|:---:|
|char |1  |0~127|%c   |字元  |
|char |1  |0~127|%d   |整數  |

{% highlight c++ linenos %}
int main() {
  char a = 'a';
  char b = 'b';
  char c = 99;
  printf("a = %c, b= %c, c = %c \n", a, b, c);
  return 0;
}
{% endhighlight %}
```
a = a, b= b, c = c 
```
### char的轉換過程
儲存'a' → 對映ASCII得到97 → 產生二進位1100001 → 儲存。<br>
讀取 二進位1100001 → 轉成十進位97 → 對映ASCII得到'a' → 顯示。<br>

### 字元變數設為數字。
第一行，直接給整數到c變數。
{% highlight c++ linenos %}
char c = 97;
printf("%d\n", c);
printf("%c\n", c);
{% endhighlight %}
```
執行結果
97
a
```

### 印出char可用%c格式字串或%d格式字串
{% highlight c++ linenos %}
char c = 'a';
printf("%d\n", c);
printf("%c\n", c);
{% endhighlight %}
```
執行結果
97
a
```

### int變數設為字元。
在c++中，可以直接給int變數賦值單引號\'\'包住的字元
{% highlight c++ linenos %}
int main() {
  int c1 = 'A';
  cout << "c1=" << c1 << endl;
	return 0;
}
{% endhighlight %}
```
執行結果
65
```

### int轉型字元
想印出字元，就把int轉型成char
{% highlight c++ linenos %}
  int c1 = 'A';
  cout << "c1=" << (char)c1 << endl;
{% endhighlight %}
```
執行結果
A
```

### 字元運算
也可字元加整數做運算。<br>
第一行，字元+2。<br>
{% highlight c++ linenos %}
  int c1 = 'A' + 2;
  cout << "c1=" << (char)c1 << endl;
{% endhighlight %}
```
執行結果
c1=C
```

{% highlight c++ linenos %}
int main() {
  char a = 'a';
  char b = 'b';
  char c = 99;
  int num = c + 100;
  printf("a = %c, b= %c, c = %c \n", a, b, c);
  printf("num = %d \n", num);
  return 0;
}
{% endhighlight %}
```
a = a, b= b, c = c 
num = 199
```

### 字元轉整數
參考<https://zh.wikipedia.org/zh-tw/ASCII>

|字元|ascii code|
|:--:|:--:|
|0|48|
|1|49|
|2|50|
|3|51|
|4|52|
|5|53|
|6|54|
|7|55|
|8|56|
|9|57|

字元\'0\'的ascii code為48

字元\'0\'到\'9\'的ascii code - 48 = 整數0到9

舉個例子，\'9\'的ascii code是57，\'0\'的ascii code為48， 57 - 48 = 整數9

而char屬於整數，可以直接用字元相減，\'9\' - \'0\' = 9

字元\'0\'到\'9\'減\'0\'，則會變成整數0-9。

{% highlight c++ linenos %}
int main() {
  int num = '9' - '0';
  cout << num << endl;
}
{% endhighlight %}

```
9
```

### 大小寫轉換

|字元|ascii code|字元|ascii code|
|:--:|:--:|:--:|:--:|
|a|97|A|65|
|b|98|B|66|
|c|99|C|67|
|d|100|D|68|
|e|101|E|69|
|f|102|F|70|


從上表可以發現小寫的ascii code與大寫的ascii code中間的差距是32。

97 - 65 = 32

98 - 66 = 32

99 - 66 = 32

#### 小寫轉大寫

{% highlight c++ linenos %}
  char upper = 'a' - 32;
  cout << upper << endl;
{% endhighlight %}
```
A
```

#### 大寫轉小寫
{% highlight c++ linenos %}
  char lower = 'A' + 32;
  cout << lower << endl;
{% endhighlight %}
```
a
```

### 判斷字母區間

判斷0-9
{% highlight c++ linenos %}
  char c = 'A';
  if (c < '0' || c > '9') cout << "不是數字" << endl;
{% endhighlight %}
```
不是數字
```

判斷a-z
{% highlight c++ linenos %}
  char c = 'A';
  if (c < 'a' || c > 'z') cout << "不是小寫字母" << endl;
{% endhighlight %}
```
不是小寫字母
```

判斷A-Z
{% highlight c++ linenos %}
  char c = 'a';
  if (c < 'A' || c > 'Z') cout << "不是大寫字母" << endl;
{% endhighlight %}
```
不是大寫字母
```
### 跳脫字元
重要有以下幾種

|Ascii碼|跳脫字元|描述
|:---:|:---:|:---|
|0|\\0|空，字元變數可設定0|
|9|\\t|TAB鍵對齊|
|10|\\n|換行|
|13|\\r|移動到最前面|


#### 設定空字元
{% highlight c++ linenos %}
  char c4 = 0;
  cout << "c4=" << c4 << endl;
{% endhighlight %}

```
執行結果
c4=
```

#### 關於斜線
因為\"與\'與\\，被編譯器作為以下用途。

* \"\"，把字串包起來。
* \'\'，把字元包起來。
* \\，換行\\n

所以不能直接使用\"與\'與\\，必須加上\\"與\\'與`\\`。

#### 雙引號
{% highlight c++ linenos %}
  char c2 = '"';
  cout << "c2=" << c2 << endl;
  cout << "我說，\"跑!\"" << endl;
{% endhighlight %}

第1行，當作字元的雙引號，前面不用加斜線`'\"'`，直接寫雙引號就可以`'"'`。

第3行，當作字串中的的雙引號，前面要加斜線。

```
執行結果
c2="
我說，"跑!"
```

#### 單引號
{% highlight c++ linenos %}
  char c3 = '\'';
  cout << "c3=" << c3 << endl;
  cout << "我說，'跑!'" << endl;
{% endhighlight %}

第1行，當作字元的單引號，前面要加斜線`'\''`。

第3行，當作字串中的的單引號，前面不用加斜線。

```
執行結果
c3='
我說，'跑!'
```

