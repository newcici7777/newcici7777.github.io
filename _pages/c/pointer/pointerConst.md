---
title: const與指標
date: 2024-05-28
keywords: c++, pointer, Pointer to Const, Const Pointer
---
以下二種十分容易混淆，而且十分難記。

## Pointer to Const (const\*)
### const右邊是星號\*
代表指標位址中的`內容`是常數，常數不能修改，以下語法不能使用。
`*指標 = 修改的內容`

{% highlight c++ linenos %}
int main() {
  int var1 = 10;
  //將var1的位址給指標變數p
  int const * p = &var1;

  //p指標位址中的內容是常數，常數不能修改，所以不能使用*指標 = 修改的內容
  //* p = 11;
  
  //印出var1的值與p指標位址中的值
  cout << "var1=" << var1 << ",p=" << * p << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
var1=10,p=10
```

`int const * p`可以更改成`const int* p`，二者為一樣的意思，只要記得const右邊若先出現星號\*而不是變數名，代表指標位址中的內容是常數，常數沒有辦法修改。

{% highlight c++ linenos %}
int main() {
  int var1 = 10;
  const int* p = &var1;
  cout << "var1=" << var1 << ",p=" << * p << endl;
  return 0;
}
{% endhighlight %}

第3行，`int const * p`可以更改成`const int* p`。

### 可以修改指標指向的記憶體位址
{% highlight c++ linenos %}
int main() {
  int var1 = 10;
  int var2 = 20;
  //將var1的位址給指標p
  const int* p = &var1;
  cout << "var1=" << var1 << ",p=" << * p << endl;
  //將var2的位址給指標p
  p = &var2;
  cout << "var2=" << var2 << ",p=" << * p << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
var1=10,p=10
var2=20,p=20
```

### const字串指標
Prerequisites:

- [字串指標][1]

```
char *c = "字串內容"
```
「c指標」指向RODATA區塊的「字串常數」，RODATA是唯讀區塊，無法修改字串常數的內容，但可以修改指標指向到新的字串常數。。<br>

![img]({{site.imgurl}}/c++/arr/char_arr4.png)<br>

### 字串指標加上const
因為指向的RODATA已經是字串常數，建議在char* 前加上const常數宣告，編譯器才不會出現警告，字串指標加上const代表無法修改RODATA的字元，但可以指向其它常數字串。<br>
下面程式碼，加上const無法修改\"abc\"常數字串，指標名c，可以指向其它新的常數字串。<br>

{% highlight c++ linenos %}
int main() {
  const char* c = "abc";
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}

RODATA區塊的字串常數無法修改。<br>
以下會編譯錯誤。<br>

{% highlight c++ linenos %}
int main() {
  constant char* c = "abc";
  c[1] = g;
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}

指標可以指向其它字串。<br>
{% highlight c++ linenos %}
int main() {
  constant char* c = "abc";
  c = "zzzz";
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}
```
zzz
```

## Const Pointer (const 指標名)
### const右邊是指標名
代表指標名是常數，需要初始化常數名(初始化設位址)，不能再更改指標指向的記憶體位址。
{% highlight c++ linenos %}
int main() {
  int var1 = 10;
  int var2 = 11;
  //const右邊是指標名p，代表指標名p是常數，常數需要初始化，不能是nullptr，將var1的位址給指標p。
  int* const p = &var1;

  //將var2的位址給指標p，會編譯失敗，因為指標p是常數，不能被修改位址。
  //p = &var2;
  cout << "var1=" << var1 << ",p=" << *p << endl;
  return 0;
}
{% endhighlight %}

### 可以修改內容
可使用`*指標 = 修改的內容`修改指標位址的值，因為只有指標名是常數，不能變更初始位址，但指標位址中的內容並非常數，所以可以改變指標位址中的內容。

{% highlight c++ linenos %}
int main() {
  int var1 = 10;
  int* const p = &var1;
  *p = 55;
  cout << "var1=" << var1 << ",p=" << *p << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
var1=55,p=55
```

[1]: {% link _pages/c/array/charArray.md %}#字串指標