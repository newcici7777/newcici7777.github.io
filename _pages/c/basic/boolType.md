---
title: bool
date: 2024-05-01
keywords: c++, bool
---
## 整數
整數包含char, `bool`, short, unsinged short, int, unsinged int, long, unsinged long

|整數型態|占用Byte數量|數值範圍|格式字串|輸出格式|
|:---:|:---:|:---:|:---:|:---:|
|bool |1  |0 1|%d   |整數  |

### 0是false，非0為true。
{% highlight c++ linenos %}
  bool b3 = '0';
  bool b4 = 0;
  cout << "b3=" << b3 << ",b4=" << b4 << endl;
{% endhighlight %}
```
執行結果
b3=1,b4=0
```

### 可使用true跟false設定bool變數
編譯器會自動把true翻譯成整數1，false翻譯成整數0。

{% highlight c++ linenos %}
  bool b1 = true, b2 = false;
  cout << "b1=" << b1 << ",b2=" << b2 << endl;
{% endhighlight %}
```
執行結果
b1=1,b2=0
```

### bool整數運算
因為bool是整數，所以bool能做整數運算。但bool顯示出來只會是0或1，即便設的值是2，顯示出來的值仍是1。
{% highlight c++ linenos %}
  bool b5 = 1;
  bool b6 = 2;
  cout << "b5 = " << b5 << ", b6 = " << b6 << endl;
  cout << "b5 + b6 = " << b5 + b6 << endl;
{% endhighlight %}
```
執行結果
b5 = 1, b6 = 1
b5 + b6 = 2
```

### cin只能輸0或1，不能輸入true或false
{% highlight c++ linenos %}
  bool b7;
  cin >> b7;
  cout << "b7 = " << b7 << endl;
{% endhighlight %}
```
執行結果
1
b7 = 1
```
```
執行結果
true
b7 = 0
```

## !相反
在迴圈與if條件判斷，會把bool真變假，假變真。<br>
下面程式碼，result原本是false，透過!相反運算子，把false變成true，因此會印出flow2。
{% highlight c++ linenos %}
int main() {
  int score = 100;
  bool result = score > 190;
  if (result) {
    cout << "flow1" << endl;
  }
  if (!result) {
    cout << "flow2" << endl;
  }
  return 0;
}
{% endhighlight %}
```
flow2
```