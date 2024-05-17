---
title: char字元
date: 2024-04-18
keywords: c++, char
---

## 整數

整數包含`char`, bool, short, unsinged short, int, unsinged int, long, unsinged long

### char

char是正整數，雖然顯示是字元，但實際存放在記憶體位置的資料型態是整數。

char所對映的整數是顯示在瑩幕上的ASCII碼。<https://zh.wikipedia.org/zh-tw/ASCII>

若輸入單引號\'\'包住的字元，編譯器會自動把單引號包住的字元轉成整數。

比如\'a\'，記憶體位址存放的十進位值是97，二進制是01100001  

              

|整數型態|占用Byte數量|數值範圍|格式符|輸出格式|
|:---:|:---:|:---:|:---:|:---:|
|char |1    |0~127|%c   |字元  |
|char |1    |0~127|%d   |整數  |


#### 字元變數賦值數字。


程式碼

{% highlight c++ linenos %}
char c = 97;
printf("%d\n", c);
printf("%c\n", c);
{% endhighlight %}

第一行，直接給整數到c變數。

```
執行結果
97
a
```

#### 印出char可用%c或%d

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

#### int變數賦值字元。

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

#### int轉型字元

想印出字元，就把int轉型成char

{% highlight c++ linenos %}
    int c1 = 'A';
    cout << "c1=" << (char)c1 << endl;
{% endhighlight %}
```

執行結果
A
```

#### 字元運算

也可字元加整數做運算。

{% highlight c++ linenos %}
    int c1 = 'A' + 2;
    cout << "c1=" << (char)c1 << endl;
{% endhighlight %}

第一行，字元+2。

```
執行結果
c1=C
```

#### 跳脫字元

重要有以下幾種

|Ascii碼|跳脫字元|描述
|:---:|:---:|:---|
|0|\\0|空，字元變數可設定0|
|9|\\t|TAB鍵對齊|
|10|\\n|換行|
|13|\\r|移動到最前面|


##### 設定空

{% highlight c++ linenos %}
    char c4 = 0;
    cout << "c4=" << c4 << endl;
{% endhighlight %}


##### 關於斜線

因為\"與\'與\\，被編譯器作為以下用途。

* \"\"，把字串包起來。
* \'\'，把字元包起來。
* \\，換行\\n

所以不能直接使用\"與\'與\\，必須加上\\"與\\'與`\\`。

##### 雙引號

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

##### 單引號

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


