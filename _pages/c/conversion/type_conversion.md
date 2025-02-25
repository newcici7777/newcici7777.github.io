---
title: c++型別轉換
date: 2025-02-24
keywords: c++, type conversion, static cast, const cast, Reinterpret cast
---

Prerequisites:
- [建構子轉型][1]
- [強制轉型][2]

以下C語言的顯式轉換

語法
```
(類型)表達式
類型(表達式)
```
{% highlight c++ linenos %}
int a = (int)8.3;
int a = int(8.3);
Student student2 = Student("student2");
Student student3 = (Student)"student2";
{% endhighlight %}

接下來要說C++的顯式轉換

## static_cast
語法
```
static_cast<類型>(表達式)
```

### 基本型態
{% highlight c++ linenos %}
int main() {
  // int是4byte
  int i = 10;
  // long在win是4byte，在linux8byte
  // 低精準度int轉成高精準度long，不會有小數點被截掉，精準度不足的問題
  long long0 = i;
  // Child child;
  double d = 5.55;
  // 隱式轉換，double是8byte高精準度轉到long4byte低精準度
  // 會有小數點被截掉，精準度不足的問題
  long long1 = d;
  // c語言的顯式轉換
  long long2 = (long)d;
  // c++的顯示轉換
  long long3 = static_cast<long>(d);
  return 0;
}
{% endhighlight %}

### 指標轉換

以下C語言的指標顯式轉換

語法
```
(指標類型*)位址
```

以下C++的指標顯式轉換，但static cast只支援同類型轉換
語法
```
static_cast<指標類型*>(位址)
```
不同類型要先轉成void指標(任何類型指標都能轉成void指標)
{% highlight c++ linenos %}
  void* point_v1 = 變數位址;
  double* 指標變數 = static_cast<類型*>(point_v1);
{% endhighlight %}

完整程式碼
{% highlight c++ linenos %}
int main() {
  int i = 10;
  // c的強制轉換
  double* point_d1 = (double*)&i;
  // c++的強制轉換
  // static cast不能轉換二種不同類型的指標
  // 以下語法編譯不過，因為&i是整數類型的指標
  //double* point_d2 = static_cast<double>(&i);
  // 需要先把整數類型的指標轉成void類型的指標
  //任何類型的指標都可以隱式轉換成void指標
  void* point_v1 = &i;
  double* point_d3 = static_cast<double*>(point_v1);
  return 0;
}
{% endhighlight %}

### 函式轉換指標類型
在函式中要轉換不同類型指標，函式參數要用void\*
{% highlight c++ linenos %}
void func(void* ptr) {
  double* point = static_cast<double*>(ptr);
}
int main() {
  int i = 10;
  func(&i);
  return 0;
}
{% endhighlight %}

## reinterpret_cast

以下三種語法例子，一定要有一方是指標，要嘛指標轉指標，要嘛指標轉成數值，要嘛數值轉成指標

### 轉換不同類型指標

語法1:不同類型指標轉換
```
reinterpret_cast<指標類型>(位址或指標)
```

轉換指標可以不用透過void指標，就直接把指標轉型。
{% highlight c++ linenos %}
int main() {
  int i = 10;
  double* point_double = reinterpret_cast<double*>(&i);
  return 0;
}
{% endhighlight %}

### 基本類型與指標互相轉換

所謂的基本類型是指一定要跟指標一樣是8byte才能互轉，不能用int(4byte)轉指標(8byte)，會編譯不過。

語法2:long long(8byte)轉成指標(8byte)
```
reinterpret_cast<指標類型>(基本類型)
```

語法3:指標轉成long long
```
reinterpret_cast<基本類型>(指標類型)
```
{% highlight c++ linenos %}
void func(void* ptr) {
  // 指標轉成long long
  long long ll = reinterpret_cast<long long>(ptr);
  cout << "ll = " << ll << endl;
}
int main() {
  long long ll = 10;
  // long long(8byte)轉成void指標(8byte)
  func(reinterpret_cast<void*>(ll));
  return 0;
}
{% endhighlight %}

## const_cast

static_cast與reinterpret_cast不能去掉const類型，但const_cast可以去掉const類型

### 基本資料型態去掉const

對於基本類型const轉成不是const類型，可以編譯成功
{% highlight c++ linenos %}
int main() {
  // 對於基本類型沒有const轉成不是const類型的問題
  const int i1 = 10;
  int i2 = i1;
  return 0;
}
{% endhighlight %}

### const指標轉成不是const指標會編譯不過
若把const指標轉成不是const指標，以下程式碼會編譯不過，不能把const類型指標指派給不是const類型指標。
{% highlight c++ linenos %}
int main() {
  const int* ptr_i1 = nullptr;
  int* ptr_i2 = ptr_i1;
  return 0;
}
{% endhighlight %}

### const_cast語法

以下C語言的指標去掉const

語法
```
(指標類型*)位址
```

以下C++的指標去掉const
語法
```
const_cast<指標類型*>(位址或指標)
```

使用const_cast強制轉型
{% highlight c++ linenos %}
int main() {
  const int* ptr_i1 = nullptr;
  // c語言去掉const，強制轉型成不是const
  int* ptr_i2 = (int*)ptr_i1;
  // c++強制轉型不是const
  int* ptr_i3 = const_cast<int*>(ptr_i1);
  return 0;
}
{% endhighlight %}

### 非const類型轉成const
{% highlight c++ linenos %}
int main() {
    char *p1 = "321";
    const char *c = const_cast<const char*>(p1);
  return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/conversion/conversion_function.md %}
[2]: {% link _pages/c/conversion/explicit.md %}