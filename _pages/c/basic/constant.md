---
title: 常數
date: 2025-07-27
keywords: c++, c, constant
---
常數是在等號(=)右邊，指派變數的值。<br>

語法
```
類型 變數 = 常數;
```

常數在程式執行時是固定不變，不可以再變更。<br>
常數的資料型態，分為以下:<br>
1. 整數常數
2. 浮點數常數
3. 字元常數
4. 字串常數
5. enum列舉

## 整數
### 十進位 二進位 八進位 十六進位
二進位0b開頭。<br>
八進位0開頭。<br>
十六進位0x或0X開頭。<br>
十進位前面沒有任何東西。<br>

{% highlight c++ linenos %}
  // 10進位
  int i10 = 5678;
  cout << "i1 = " << i10 << endl;
  // 2進位
  int i2 = 0b0001;
  cout << "i2 = " << i2 << endl;
  // 8進位
  int i8 = 010;
  cout << "i8 = " << i8 << endl;
  // 16進位
  int i16 = 0x10;
  cout << "i16 = " << i16 << endl;
{% endhighlight %}
```
i1 = 5678
i2 = 1
i8 = 8
i16 = 16
```

### unsigned無符號常數
字尾加上u或U，就是unsigned。<br>
{% highlight c++ linenos %}
  unsigned int ui1 = 4294967295u;
  cout << "ui1 = " << ui1 << endl;
{% endhighlight %}
```
ui1 = 4294967295
```

如果字尾沒加上u，視為整數常數，進行自動轉型成unsigned int。<br>
{% highlight c++ linenos %}
  unsigned int ui1 = 4294967295;
{% endhighlight %}

### long常數
字尾加上l或L，就是長整數。
{% highlight c++ linenos %}
long iL1 = 10L;
{% endhighlight %}

### long long常數
字尾加上ll或LL。
{% highlight c++ linenos %}
long long iLL1 = 10LL;
{% endhighlight %}

### unsigned long常數
字尾加上ul或UL。
{% highlight c++ linenos %}
unsigned long iUL1 = 10UL;
{% endhighlight %}

### 表格

|整數常數|字尾|整數型態|
|123  |沒有|整數|
|123u |u或U|無符號unsigned int|
|123l |l或L|長整數long|
|123ll|ll或LL|long long|
|123ul|ul或UL|無符號長整數unsigned long|
|123ull|ull或ULL|unsigned long long|

## 字元常數
字元常數以\'\'符號包住字元。
{% highlight c++ linenos %}
char c = 'A';
{% endhighlight %}

跳脫字元也是字元常數
{% highlight c++ linenos %}
char c = '\t';
{% endhighlight %}

## 字串常數
{% highlight c++ linenos %}
char str[] = "Hello World!";
cout << str << endl;
{% endhighlight %}
```
Hello World!
```

### 字串斷行
若字串太長，可使用`\`來斷行，執行結果跟上面的程式碼一樣。
{% highlight c++ linenos %}
  char str[] = "Hello \
World!";
  cout << str << endl;
{% endhighlight %}
```
Hello World!
```

## constant
在main()主程式以前，設定constant類型、常數名與值，後面(尾部)有分號`;`。<br>
語法
```
constant 類型 常數名 = 值;
int main() {
  return 0;
}
```

{% highlight c++ linenos %}
const double PI = 3.14159;
int main() {
  cout << PI << endl;
  return 0;
}
{% endhighlight %}

### 無法修改constant
以下程式碼會編譯錯誤，常數不可再被修改。
{% highlight c++ linenos %}
const double PI = 3.14159;
int main() {
  PI = 2.55;
  cout << PI << endl;
  return 0;
}
{% endhighlight %}

### 無法重覆宣告constant
以下程式碼會編譯錯誤，常數不可重覆宣告。
{% highlight c++ linenos %}
const double PI = 3.14159;
const double PI = 1.666;
int main() {
  cout << PI << endl;
  return 0;
}
{% endhighlight %}