---
title: 指派基本型態與指派指標
date: 2025-07-30
keywords: c++, passed by value, a pointer passed by value
---
## 指派基本型態 pass by value
基本型態(int, short, byte, long, long long, char)變數指派，都是passed by value，也就是把變數的「值」，複製到其它變數的記憶體位址的值中。

{% highlight c++ linenos %}
int i = 55;
int j = i; // 把i指派給j
{% endhighlight %}

![img]({{site.imgurl}}/pointer/pass_value.png)

### 基本型態修改值
把j變數的值，從55修改成40，不會影嚮變數i，因為i與j的記憶體空間是各別獨立，不會互相影嚮。
{% highlight c++ linenos %}
int i = 55;
int j = i; // 把i指派給j
j = 40;
{% endhighlight %}

![img]({{site.imgurl}}/pointer/pass_value2.png)

## 指派指標 a pointer passed by value
指派給指標的值是「記憶體位址」，不是值，以下程式碼，變數p的值就是記憶體位址，所以可以指派給p2變數。
{% highlight c++ linenos %}
int main() {
  int i = 55;
  int* p = &i;
  int* p2 = p;
  cout << "i 位址 = " << &i << endl;
  cout << "p 變數的值 = " << p << endl;
  cout << "p2 變數的值 = " << p2 << endl;
  return 0;
}
{% endhighlight %}
```
i 位址 = 0x00000008 
p 變數的值 = 0x00000008
p2 變數的值 = 0x00000008
```

p是p變數的「值」。<br>
把p變數的「值」，儲存在p2的記憶體位址中，p2的值也跟p的值一樣。<br>
下圖中，p2變數也有自己的記憶體位址0x00000000E，但它儲存的是0x00000008。<br>

![img]({{site.imgurl}}/pointer/pointer_memory4.png)

### 指標修改值
指標修改值，會影嚮其它指標與原本的變數。

假如修改p2
```
*p2 = 100;
```

![img]({{site.imgurl}}/pointer/pass_address1.png)

{% highlight c++ linenos %}
int main() {
  int i = 55;
  int* p = &i;
  int* p2 = p;
  * p2 = 100;
  cout << "i  = " << i << endl;
  cout << "* p  = " << * p << endl;
  cout << "* p2  = " << * p2 << endl;
  return 0;
}
{% endhighlight %}
```
i  = 100
* p  = 100
* p2  = 100
```

## 結論
其實基本型態與指標的指派，都是複製記憶體位址的「值」，給另一個變數。<br>
只是傳值，是基本型態的值的複製，修改不會影嚮其它變數。<br>
傳記憶體位址，是記憶體位址的複製，修改會影嚮其它變數。<br>
