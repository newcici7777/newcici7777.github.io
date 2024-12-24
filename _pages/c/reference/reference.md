---
title: 參考
date: 2024-06-20
keywords: c++, reference
---

## 宣告方法

```
資料型態& 參考別名 = 原始變數;
```

## 注意事項

- 宣告參考別名時，要初始化原始變數。

- 初始化後不可改變原始變數。

- 別名可以存取原始變數，參考別名與原始變數都是指向相同記憶體位址。

## 宣告

{% highlight c++ linenos %}
  int i = 10;//原始變數
  //宣告參考
  int& ref = i;//初始化原始變數，ref是i的別名
  cout << "i的記憶體位址 = " << &i << ", i = " << i << endl;
  cout << "ref的記憶體位址 = " << &ref << ", ref = " << ref << endl;
{% endhighlight %}

```
i的記憶體位址 = 0x7ff7bfeff468, i = 10
ref的記憶體位址 = 0x7ff7bfeff468, ref = 10
```
由執行結果可知，i跟ref的記憶體位址相同，值也相同。

## 一定要初始化

以下的程式寫法會編譯錯誤，因為ref參考別名沒有初始化。

{% highlight c++ linenos %}
  int i = 10;//原始變數
  //宣告參考
  int& ref;
{% endhighlight %}

## 參考別名跟原始變數功能一樣，可以修改跟讀取記憶體內容。

參考別名跟原始變數都指向相同記憶體位址，所以可以做一樣的操作。

{% highlight c++ linenos %}
  int i = 10;//原始變數
  //宣告參考
  int& ref = i;
  cout << "i的記憶體位址 = " << &i << ", i = " << i << endl;
  cout << "ref的記憶體位址 = " << &ref << ", ref = " << ref << endl;
  
  ref = 30;
  cout << "i的記憶體位址 = " << &i << ", i = " << i << endl;
  cout << "ref的記憶體位址 = " << &ref << ", ref = " << ref << endl;
{% endhighlight %}

```
i的記憶體位址 = 0x7ff7bfeff468, i = 10
ref的記憶體位址 = 0x7ff7bfeff468, ref = 10
i的記憶體位址 = 0x7ff7bfeff468, i = 30
ref的記憶體位址 = 0x7ff7bfeff468, ref = 30
```

由執行結果可以發現修改ref參考別名，就等於修改變數i，i跟ref的值都變成30。

## 一個變數可以多個參考別名

{% highlight c++ linenos %}
  int i = 10;//原始變數
  //宣告參考
  int& ref = i;
  cout << "i的記憶體位址 = " << &i << ", i = " << i << endl;
  cout << "ref的記憶體位址 = " << &ref << ", ref = " << ref << endl;
  
  int& ref2 = i;
  cout << "ref2的記憶體位址 = " << &ref2 << ", ref2 = " << ref2 << endl;
  int& ref3 = i;
  cout << "ref3的記憶體位址 = " << &ref3 << ", ref3 = " << ref3 << endl;
{% endhighlight %}

```
i的記憶體位址 = 0x7ff7bfeff468, i = 10
ref的記憶體位址 = 0x7ff7bfeff468, ref = 10
ref2的記憶體位址 = 0x7ff7bfeff468, ref2 = 10
ref3的記憶體位址 = 0x7ff7bfeff468, ref3 = 10
```

## 各種寫法

&只要介於`資料型態`與`參考別名`之間就可以，以下的宣告別名的方式都可以。

{% highlight c++ linenos %}
  int i = 10;//原始變數
  //宣告參考別名
  int&ref = i;
  int &ref2 = i;
  int & ref3 = i;
  int& ref4 = i;
{% endhighlight %}

## 資料型態要一致，參考別名無強制轉型

以下是錯誤的參考別名宣告

{% highlight c++ linenos %}
  int i = 10;//原始變數
  //宣告參考別名
  double& ref = i;
{% endhighlight %}

原始變數i是int，無法轉成double的資料型態，也沒辦法像指標有強制轉型的功能。

## 初始化後不可改變原始變數

{% highlight c++ linenos %}
  int i = 10;//原始變數
  //宣告參考
  int& ref = i;
  cout << "i的記憶體位址 = " << &i << ", i = " << i << endl;
  cout << "ref的記憶體位址 = " << &ref << ", ref = " << ref << endl;
  
  int j = 20;
  ref = j; //將j變數 指派給 ref別名
  cout << "j的記憶體位址 = " << &j << ", j = " << j << endl;
  cout << "ref的記憶體位址 = " << &ref << ", i = " << ref << endl;
  cout << "i的記憶體位址 = " << &i << ", i = " << i << endl;
{% endhighlight %}

上方的程式碼

```
  int j = 20;
  ref = j; //將j變數的值 指派給 ref參考別名
```

以上意思並非把原始變數設為j，實際上是變數i修改成20，ref是變數i的參考別名，就等同於變數i，所以上述程式碼可以看成`i = 20;`

```
執行結果
i的記憶體位址 = 0x7ff7bfeff468, i = 10
ref的記憶體位址 = 0x7ff7bfeff468, ref = 10
j的記憶體位址 = 0x7ff7bfeff45c, j = 20
ref的記憶體位址 = 0x7ff7bfeff468, i = 20
i的記憶體位址 = 0x7ff7bfeff468, i = 20
```
由執行結果可以發現，執行`ref = j;`參考別名j的記憶體位址跟i相同，但j的值變成20，i的值也變成20，修改ref就等同於修改i，所謂的參考別名就是給變數i另一個假名ref，但指向的都是相同記憶體位址。





